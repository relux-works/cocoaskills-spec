# Independent review evidence

Stable protocol releases require two public review reports under
`reviews/<version>/`:

- `security.json` for the registry and manager security model;
- `interoperability.json` for independent implementation and shared-suite
  interoperability.

Each report conforms to `review-report.schema.json`. The reviewer must not be a
maintainer of this specification or an author or committer of the normative
changes under review. Affiliation and any relevant conflict of interest are
disclosed in the report. The `source_url` points to the public review, issue, or
pull request that establishes reviewer authorship and discussion history.

Reviewers inspect a stable-version candidate commit before reports are added.
After that commit, only files below `reviews/` may change before the release
tag. Release CI verifies ancestry and this diff restriction for both reports.
Open critical or high findings, a conditional conclusion, and a failed report
all block the stable release.

The examples directory is validation input, not release evidence.

