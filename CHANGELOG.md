# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- New argument `base_branch` in `draft-new-release` (default to `main`).

### Fixed

- Specify toolchain in `draft-new-release` to be _stable_.

## [1.1.0] - 2022-05-27

### Added

- New input `build` for `draft-new-release.yml` to build the crate before add & commit the files, allows lockfile update
- New input `check_publish` for `draft-new-release.yml` to dry run the cargo publish before add & commit the files, allows to not open a PR on unpublishable state.

### Changed

- Update `actions/checkout@v3`, `actions/download-artifact@v3`, `thomaseizinger/create-pull-request@1.2.2`, `EndBug/add-and-commit@v9.0.0`, `softprops/action-gh-release@v1`
- Bump header's year to 2022

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

[Unreleased]: https://github.com/farcaster-project/workflows/compare/v1.1.0...HEAD
[1.1.0]: https://github.com/farcaster-project/workflows/compare/v1.0.2...v1.1.0
[1.0.2]: https://github.com/farcaster-project/workflows/compare/v1.0.1...v1.0.2
[1.0.1]: https://github.com/farcaster-project/workflows/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/farcaster-project/workflows/compare/0c88c46cfe1d25098ec47216e4b2dfc8bf871338...v1.0.0
