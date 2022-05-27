# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- New input `build` for `draft-new-release.yml` to build the crate before add & commit the files, allows lockfile update
- New input `check_publish` for `draft-new-release.yml` to dry run the cargo publish before add & commit the files, allows to not open a PR on unpublishable state.

## [1.0.2] - 2021-11-01

### Added

- Add workflow call inputs on `create-release.yml` workflow to pass an artifact containing files to attached to the release

### Changed

- Improved documentation on how to chain and trigger workflows

## [1.0.1] - 2021-10-29

### Fixed

- Modify `Cargo.toml` version based on received version input

### Added

- Format checks on markdown and toml files
- Usage example for each workflow

## [1.0.0] - 2021-10-29

### Added

- `Crate release` shared workflow
- `Draft new release` shared workflow
- `Release to crates.io` shared workflow

[Unreleased]: https://github.com/farcaster-project/workflows/compare/v1.0.2...HEAD
[1.0.2]: https://github.com/farcaster-project/workflows/compare/v1.0.1...v1.0.2
[1.0.1]: https://github.com/farcaster-project/workflows/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/farcaster-project/workflows/compare/0c88c46cfe1d25098ec47216e4b2dfc8bf871338...v1.0.0
