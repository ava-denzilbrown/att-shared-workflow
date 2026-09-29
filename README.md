# att-shared-workflow

Reusable GitHub Actions workflows for repositories in an organization.

## Reusable CI workflow

The `CI` workflow in `.github/workflows/ci.yml` checks out the calling
repository and runs its validation commands. The commands are provided by the
caller so this workflow can be used by repositories with different languages
and build tools.

Add a caller workflow such as `.github/workflows/ci.yml` to a consuming
repository:

```yaml
name: CI

on:
  push:
  pull_request:

permissions:
  contents: read

jobs:
  validate:
    uses: ava-denzilbrown/att-shared-workflow/.github/workflows/ci.yml@v1
    with:
      commands: |
        npm ci
        npm test
```

Replace the example commands with the checks appropriate for the calling
repository. The reusable workflow also accepts `runs_on` (default
`ubuntu-latest`) and `timeout_minutes` (default `15`). The caller must grant
the permissions the reusable workflow needs; currently that is only
`contents: read`.

Use a reviewed release tag or commit SHA in `uses` when consuming this
workflow. Commands are executed in the calling repository, so only pass
trusted workflow configuration as `commands`.
