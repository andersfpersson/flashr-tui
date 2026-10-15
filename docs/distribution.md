# Distribution Guide

This project ships prebuilt binaries primarily through GitHub Releases.

## 1) Triggering a release

1. Update `CHANGELOG.md` for the new version.
2. Tag the release in git using `vX.Y.Z`.
3. Push the tag.

The release workflow builds and packages these targets:
- `x86_64-unknown-linux-gnu`
- `aarch64-unknown-linux-gnu`
- `x86_64-apple-darwin`
- `aarch64-apple-darwin`
- `x86_64-pc-windows-msvc`

It then publishes:
- platform archives (`.tar.gz` / `.zip`)
- `SHA256SUMS.txt`
- optional GPG signature (`SHA256SUMS.txt.asc`) when `GPG_PRIVATE_KEY` and `GPG_PASSPHRASE` are set
- optional cosign signature (`SHA256SUMS.txt.cosign.sig`) when `COSIGN_PRIVATE_KEY` and `COSIGN_PASSWORD` are set
- SBOM (`flashr-tui-sbom.spdx.json`)

## 2) Crates.io (`cargo install`)

`cargo install flashr-tui` is a source build channel.

Release checklist for crates.io:
1. Ensure version in `Cargo.toml` matches release.
2. Run `cargo publish` from a trusted environment.
3. Confirm install with `cargo install flashr-tui --version X.Y.Z`.

## 3) Package-manager channels

Use GitHub Release archives and checksums as canonical inputs.

### Homebrew tap
- Maintain formula in a tap repository (for example `homebrew-tap`).
- Point `url` to GitHub Release asset.
- Set `sha256` from `SHA256SUMS.txt`.

### AUR
- Maintain `PKGBUILD` in AUR.
- Use release tarball URL + checksum.
- Bump `pkgver`, regenerate `.SRCINFO` each release.

### Scoop
- Maintain bucket manifest JSON.
- Update `version`, `url`, `hash`, and extraction/install paths.

### winget
- Submit/update manifests in `microsoft/winget-pkgs`.
- Reference installer URL and SHA256 from release assets.

### Nixpkgs
- Update package expression with new version/hash.
- Open PR to `NixOS/nixpkgs`.

## 4) Additional mirrors

Maintain a download page (project website) linking to GitHub Releases.
Optional mirrors:
- SourceForge
- Cloudflare R2
- Amazon S3

Keep file names and checksums identical to GitHub release assets.
