[![Workflow version](https://img.shields.io/badge/Workflow%20version-@v1.1.0-blue)](https://github.com/farcaster-project/workflows/releases/tag/v1.1.0)
[![License: MIT or Apache](https://img.shields.io/badge/License-MIT%20or%20Apache%202.0-yellow.svg)](./COPYRIGHT)

# Shared Workflows

This project gathers a list of workflows shared across multiple **@farcaster-project** repositories.

## Usage

You can find an example of usage for the following shared workflows.

### Draft new release

`draft-new-release.yml` prepare the creation of a new release by creating a new branch `release/{version}` and opening a pull request targeting `main`.

**The changelog is updated to `{version}` and today's date. `Cargo.toml` version is updated to `{version}` and `Cargo.toml` is updated if input `build` (default false) is set to true. Set input `check_publish` (default false) to dry run publish**.

> Version should follow semantic versioning.

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
    uses: farcaster-project/workflows/.github/workflows/draft-new-release.yml@v1.1.0
    with:
      version: ${{ github.event.inputs.version }}
      build: false
      check_publish: true
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
    uses: farcaster-project/workflows/.github/workflows/create-release.yml@v1.1.0
```

If you want to attached files to the release you can declare a `create_release` job with:

```yaml
jobs:
  produce_binaries:
    name: Compile released binaries
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Compile released binaries
        run: ...

      - name: Archive release folder
        uses: actions/upload-artifact@v2
        with:
          name: release-folder
          path: target/release
          retention-days: 7

  create_release:
    name: Create from merged release branch
    if: github.event.pull_request.merged == true && startsWith(github.event.pull_request.head.ref, 'release/')
    uses: farcaster-project/workflows/.github/workflows/create-release.yml@v1.1.0
    needs: produce_binaries
    with:
      artifact_name: release-folder
      files: |
         target/release/bin1
         target/release/bin2
```

You can add another job after `create_release`, e.g. `release_to_crates`, triggered only when the first is successfully completed i.e. when the release is published with:

```yaml
release_to_crates:
  name: Release to crates.io
  uses: farcaster-project/workflows/.github/workflows/release-to-crates-io.yml@v1.1.0
  needs: create_release
  secrets:
    cratesio_token: ${{ secrets.CARGO_REGISTRY_TOKEN }}
```

Or use the method below with `workflow_run: workflows: ["Name of the worflow"]` to keep separated files.

### Release to crates.io

`release-to-crates-io.yml` publish the crate to crates.io under the authentified user by **cratesio_token**. Example of stand-alone usage (see above for chained usage after `create_release`):

```yaml
name: Release to crates.io

on:
  release:
    types: [ created ]

jobs:
  release:
    name: "Publish the new release to crates.io"
    uses: farcaster-project/workflows/.github/workflows/release-to-crates-io.yml@v1.1.0
    secrets:
      cratesio_token: ${{ secrets.CARGO_REGISTRY_TOKEN }}
```

If you want to trigger this workflow after another you can add:

```yaml
# Trigger when the "Create release" workflow succeeded
on:
  workflow_run:
    workflows: ["Create release"]
    types:
      - completed
```

Where `"Create release"` is the name of the workflow that will trigger the `"Release to crates.io"` workflow when completed.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for detailed version logs.

## License

Licensed under either of

- Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or
  http://www.apache.org/licenses/LICENSE-2.0)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.
