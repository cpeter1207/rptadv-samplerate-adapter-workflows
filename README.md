# rptadv-samplerate-adapter workflows

This repository owns reusable GitHub Actions workflows for
[`rptadv-samplerate-adapter`](https://github.com/cpeter1207/rptadv-samplerate-adapter).
The source repository uses thin callers that follow this repository's `main`
branch, so a validated workflow repair does not require a source change.

- `preflight.yml` runs fast formatting, lint, and static analysis for source
  pushes.
- `quality.yml` is the pull-request gate: one platform-independent job, then
  native Debian 13 amd64 and arm64 verification, with production coverage on
  amd64 only.
- `quality-image.yml` publishes the native multi-architecture GHCR quality
  image from the source repository's Dockerfile.
- `documentation.yml` publishes warning-free API documentation to GitHub
  Pages.
- `release.yml` verifies the latest successful required quality check on the
  merged pull request's head, then publishes only Debian 13 packages and the
  source archive from the tagged main revision.

`validate.yml` runs Actionlint only. This keeps workflow repairs independently
possible when source quality is failing.

The expected image is
`ghcr.io/cpeter1207/rptadv-samplerate-adapter-ci:latest`. It must be
published once before container-based source gates can run.

Source callers grant elevated permissions only to the workflow that needs
them: `packages: write` for image publishing, `pages: write` plus `id-token:
write` for documentation, and `contents: write`, `checks: read`, and
`pull-requests: read` for a release. The release workflow requires the tag to
identify the exact merged main commit and its merged pull request's head to
expose a successful latest `Required quality gate` check run.
