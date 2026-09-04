# Gamescope OGC Builds

Prebuilt Arch Linux packages for the five [OpenGamingCollective](https://github.com/OpenGamingCollective) gamescope projects. This repo watches those upstream repositories, builds each one from a pinned commit, and publishes the resulting `*.pkg.tar.zst` binaries as GitHub release assets for the `ogc` pacman repository to consume.

## Packages

| Package | What it is | Tracks |
|---|---|---|
| `gamescope-ogc` | The OGC fork of Valve's gamescope compositor | `OpenGamingCollective/gamescope` `ogc` |
| `gamescope-session` | Common gamescope session framework | `gamescope-session` `main` |
| `gamescope-session-steam` | Steam Big Picture session | `gamescope-session-steam` `main` |
| `gamescope-session-ogui-steam` | Steam + OpenGamepadUI overlay session | `gamescope-session-ogui-steam` `main` |
| `gamescope-session-opengamepadui` | Standalone OpenGamepadUI session | `gamescope-session-opengamepadui` `main` |

## Installation

Install the `ogc` repository (which consumes these releases), then install the software directly:

```sh
sudo pacman -S gamescope-ogc gamescope-session-steam
```

- `gamescope-ogc` ships `provides=gamescope` and conflicts with the official `gamescope` and AUR `gamescope-git`, so you cannot have both installed.
- The session packages depend on `gamescope-ogc`, so a full session install always resolves against the same repository build — there is no silent fallback to the `extra` `gamescope`.

If you do not use the `ogc` repository, each release's archives can be downloaded from the [Releases](https://github.com/OpenGamingCollective/gamescope-builds/releases) page and installed with `pacman -U`.

## Versioning

Versions follow `r<commit-count>.<short-sha>`, matching the AUR VCS convention, e.g. `r339.b5c2d0d`. Every build is made reproducibly from an exact pinned commit, so the package version always encodes exactly which upstream source produced it, and filenames always roll whenever an upstream branch moves. Releases are tagged `v<YYYYMMDD>.<n>`, and the release notes list, for each package, the source repository and commit SHA it was built from.

## Building from source

The build runs on a daily schedule (plus manual dispatch) inside an `archlinux/archlinux:base-devel` container. See `.github/workflows/build.yml` and the `packages/` directory for the PKGBUILDs if you want to build locally:

```sh
cp -a packages/<pkg> . && cd <pkg>
sed -i 's/^_commit=.*/_commit=<sha>/' PKGBUILD
makepkg -si
```

## Project status

- x86_64 only, matching the collection pipeline's supported architecture.
- The upstream repositories publish no releases of their own; this repo supplies that artifact.
