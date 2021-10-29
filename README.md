[![Workflow version](https://img.shields.io/badge/Workflow%20version-@v1-blue)](https://docs.github.com/en/actions/learn-github-actions/reusing-workflows#calling-a-reusable-workflow)
[![License: MIT or Apache](https://img.shields.io/badge/License-MIT%20or%20Apache%202.0-yellow.svg)](./COPYRIGHT)

# Shared Workflows

This project gathers a list of workflows shared across multiple **@farcaster-project** repositories.

## Usage

You can find an example of usage for the following shared workflows.

### Draft new release

`draft-new-release.yml` prepare the creation of a new release by creating a new branch `release/{version}` and opening a pull request targeting `main`. **The changelog is updated with the `{version}` parameter and today's date and the `Cargo.toml` version is updated with the new `{version}`**. Version should follow semantic versioning.

The commit will be associated with GitHub Actions bot.

Example of usage:

```yaml
name: Draft new release

on:
  workflow_dispatch:
    inputs:
      version:
        description: 'The new version in major.minor.patch format.'
        required: true

jobs:
  draft-new-release:
    name: "Draft a new release"
    uses: farcaster-project/workflows/.github/workflows/draft-new-release.yml@v1.0.0
    with:
      version: ${{ github.event.inputs.version }}
```

### Create release

`create-release.yml` allows to create a GitHub release when merging a branch starting with `release/` (this also creates a tag `v{versionn}`).

The release will be associated with GitHub Actions bot.

Example of usage:

```yaml
name: Create release

on:
  pull_request:
    types:
      - closed

jobs:
  create_release:
    name: Create from merged release branch
    if: github.event.pull_request.merged == true && startsWith(github.event.pull_request.head.ref, 'release/')
    uses: farcaster-project/workflows/.github/workflows/create-release.yml@v1.0.0
```

### Release to crates.io

`release-to-crates-io.yml` publish the crate to crates.io under the authentified user by **cratesio_token**. Example of usage:

```yaml
name: Release to crates.io

on:
  release:
    types: [ created ]

jobs:
  release:
    name: "Publish the new release to crates.io"
    uses: farcaster-project/workflows/.github/workflows/release-to-crates-io.yml@v1.0.0
    secrets:
      cratesio_token: ${{ secrets.CARGO_REGISTRY_TOKEN }}
```

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for detailed version logs.

## License

Licensed under either of

- Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or
  http://www.apache.org/licenses/LICENSE-2.0)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.
