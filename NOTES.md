# Spec refresh working notes (non-normative)

Working document for the 0.1.0-draft to 1.0.0-draft refresh. Tracks the gap
between the original pre-implementation spec and the shipped CocoaSkills
reference implementation (v0.12.0, github.com/ivanopcode/cocoaskills, main
fdf6bc3). Removed before the refresh is declared complete, or kept as an
explicitly non-normative changelog of the rewrite.

## Ground truth

| Source | Role |
|--------|------|
| `src/csk/` @ v0.12.0 | Normative behavior. Anything absent here is not normative. |
| `docs/skill-authoring.md` | Skill format details, schemas 1-5 |
| `docs/v0.9-design.md` (RFC 0007) | Skill dependencies, closure, activation modes, dev substitutions, allowed_sources |
| `docs/v0.11-design.md` (RFC 0008) | Audit registry protocol, snapshots, system config |
| `CHANGELOG.md` | Feature timeline v0.1.x to v0.12.0 |
| github.com/ivanopcode/cocoaskills-registry | Registry server: signing, hash-chained log, HTTP API, bundles |

## Gap map: old section to new fate

| Old (0.1.0-draft) | Reality in v0.12.0 | Fate |
|---|---|---|
| 4. Skillspec.yml manifest | Does not exist. Real manifest is `csk-skill.json` schemas 1-5 | Rewrite as csk-skill.json |
| 4.4 Environment requirements | `dependencies.commands` type system | Fold into manifest section |
| 4.6-4.7 Executable distribution, build targets | No build phase. `runtime_roots` copied to runtime store; shims | Rewrite; build to Future work |
| 5. Skillfile (constraints, trust, policy, context mode, install method) | Skillfile.json schema 1 is minimal: project.alias, agents, locale, skills[] with name/git + tag or branch or revision | Rewrite; extras to Future work |
| 5.3 Version constraints (^, ~, ranges) | Not implemented. Exact refs only; branch allowed at top level, forbidden in skill-to-skill requirements | Future work |
| 6. Skillfile.lock | Not implemented. Reproducibility via exact refs, install markers, content hash | Future work |
| 7. Install process (resolve/fetch/audit/verify/build/install/adapt) | Real pipeline: fetch, dev substitution, closure resolution, source allowlist gate, audit gate, registry attestation gate, install, adapters, markers | Rewrite |
| 7.8 Stripped installation | Implemented as context/runtime split (context dirs vs runtime_roots) | Rewrite |
| 8. Multi-agent delivery | adapters.py: claude_code, codex_cli, cursor, gemini, opencode, windsurf (last two native-discovery, no project mirror) | Rewrite |
| 9. Source audit (language tiers, Makefile rules) | audit/ package: pipeline, policy, detectors, capabilities vs declared, canary, redaction, trust, backends (null, command, codex LLM) | Rewrite from code |
| 10-13. SSH cert signing, CA hierarchy, trust providers, revocation.yml | Not implemented. Replaced by RFC 0008 audit registry: Ed25519 signed audited/revoked records, pinned registry keys, deny-wins federation, signed snapshots, TTL cache + offline grace, enforced system config with locked keys, strict policy, attestations, publish, registry server with hash-chained log and air-gap bundles | Rewrite as Audit Registry Protocol; SSH PKI to Future work |
| 14. Shell integration (direnv, env.sh) | shell_init.py: zsh, bash, powershell hooks; PATH order project > global > system | Rewrite from code |
| 15. CLI (ca *, skill sign, etc.) | Real CLI: bootstrap, init, add, install, status (+ --attest), update, upgrade, gc, global *, hybrid *, skill check, shell-init, audit (+ --publish) | Rewrite |
| 16. CI/CD example | Still valid in outline | Refresh lightly |
| 17. Compatibility | SKILL.md profile statement stands; registries section superseded by RFC 0008 | Rewrite |
| 18. Version scope v0.1 | Obsolete | Replace with Conformance section |
| 19. Go implementation | Reference implementation is Python 3.11+, stdlib-only runtime (vendored Ed25519) | Rewrite |
| Missing entirely in old spec | capabilities declarations (schema 3), skill-to-skill dependencies + activation modes (schema 4), MCP server requirements + config surfaces (schema 5), locales + skill_triggers, global and hybrid scopes with shadowing, dev substitutions, source identity canonicalization, install markers, runtime store keyed by commit, gc, locking, registry server | New sections |

## Target structure (new document)

1. Overview
2. Terminology
3. Architecture and directory layout
4. Skill package format (SKILL.md profile, context directories, locales, triggers)
5. csk-skill.json manifest (schemas 1-5)
6. Skillfile project manifest and dev substitutions
7. Machine and system configuration (config.json, allowed_sources, locked keys)
8. Resolution and installation (closure, gates, markers, content hash, runtime store, shims)
9. Install scopes: project, global, hybrid; shadowing
10. Multi-agent delivery (adapters)
11. MCP server requirements
12. Source audit
13. Audit registry protocol (client and server)
14. Shell integration
15. CLI reference (informative)
16. Compatibility
17. Conformance for independent implementations
Appendix A: Future work (SSH cert PKI, CA hierarchy, version ranges, lockfile, trust providers)
Appendix B: Field reference tables
References, About Relux Works (kept)

## Pass plan

- P1: this file; metadata + abstract refresh. (this commit)
- P2: sections 4-5 (skill package format, csk-skill.json).
- P3: sections 6-11 (Skillfile, config, install semantics, scopes, adapters, MCP).
- P4: sections 12-13 (audit, registry protocol + server).
- P5: sections 1-3, 14-17, appendices, TOC, style pass.
- P6: consistency gate against src/csk; fix commit; decide fate of NOTES.md.
