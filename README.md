# GWTrade CI

Public CI harness for the private GWTrade source repository:
https://github.com/greenways-ai/gwtrade

## Triggering

The gwtrade repository runs .github/workflows/notify.yml on pushes to main. It
sends a code-base-changed repository-dispatch event containing the exact source
repository, ref, and commit SHA.

The CI workflow also supports manual reruns. Start GWTrade CI with the
40-character source_sha to test an existing source revision without relying on
a moving branch.

## GitHub App setup

Install one GitHub App on both greenways-ai/gwtrade and greenways-ai/gwtrade-ci.
The App must be able to mint:

- Contents: write for the gwtrade-ci repository, for repository dispatch.
- Contents: read for the gwtrade repository, for source checkout.

Configure these names in both repositories (or at organization scope):

- Variable: GWTRADE_CI_APP_ID
- Secret: GWTRADE_CI_APP_PRIVATE_KEY

The workflows scope each short-lived token to only the repository it needs. No
source credentials are included in the dispatch payload.

## Toolchain and database

The workflow checks out Hara Foundation revision
a706cd91d82ddce3003d75a5d0edc9178c6cc6ef, verifies Hara Native 0.1.21, builds
and installs Foundation into an isolated HARA_DIST_HOME, and starts PostgreSQL
16 on port 55222 with the required extensions.
