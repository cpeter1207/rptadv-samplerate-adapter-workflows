# Workflow development rules

This repository owns reusable GitHub Actions implementations for
`rptadv-samplerate-adapter`. It contains no production audio code.

Validate workflow-only changes with Actionlint only. Keep that validation
independent of production quality so a broken workflow can be repaired without
requiring an unrelated source build to pass. Workflows must not rewrite source
files or depend on a node.

For the adapter source repository, ordinary pushes run formatting, lint, and
static analysis once. Pull requests run formatting, lint, static analysis, and
Doxygen once, then native Debian 13 amd64 and arm64 build, test, package, and
staged-install jobs in parallel. Production line and branch coverage is
required only on Debian 13 amd64. Debian 12 is not automated. Releases build
only source and Debian 13 package artifacts from a main revision that already
has a successful required-quality check; they do not repeat the full gate.

Publish the quality image natively for amd64 and arm64 as one GHCR manifest.
The source repository owns deterministic cleanup of its labeled test
containers. Never deploy to a node or alter node configuration without
explicit approval.
