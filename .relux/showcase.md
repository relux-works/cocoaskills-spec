---
title: CocoaSkills
summary: An enterprise-grade package manager and security PKI for AI agent skills.
category: Agent infrastructure
featured: true
site: https://cocoaskills.org
---

## What it is

CocoaSkills is a dependency manager for AI agent skills — the reusable instruction
packages that give coding agents specialized capabilities. It brings to a young,
fast-moving ecosystem the tooling that mature package managers (Bundler, SPM, Cargo,
Gradle) take for granted: declarative manifests, reproducible installs, and version
pinning.

## Why it matters

Skills are a new embodiment of source code. Even a plain-markdown skill can carry a
prompt-injection payload, and as skills grow they pull in binaries that demand real
supply-chain security. Content scanning alone cannot catch publisher impersonation,
artifact tampering, silent mutation inside a pinned version, or post-install
substitution — those require public-key cryptography. CocoaSkills closes that gap with
an SSH-certificate signing model, hierarchical CA trust, and pluggable identity
verification: a PKI purpose-built for agent-skill artifacts, working across both public
and self-hosted registries.

## Who it is for

Teams that run agents in production and need their skill supply chain to be reproducible,
attestable, and auditable — the same rigor they already apply to code dependencies.
