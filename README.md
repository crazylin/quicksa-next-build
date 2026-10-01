# quicksa-next-build

CI runner for the private `crazylin/quicksa-next-poc` repository.

This repository contains only a GitHub Actions workflow. It has no source
code. The workflow checks out `quicksa-next-poc` at run time with a
read-only deploy key (`SOURCE_DEPLOY_KEY`) and builds the desktop app and
headless test suite on Ubuntu, macOS, and Windows.

Run it manually:

```sh
gh workflow run build.yml -R crazylin/quicksa-next-build -f ref=main
```

Run logs of a public repository are public. `build.yml` uploads no build
artifacts.

`build.yml` caches build work in this repository's Actions cache:

- Windows: the vcpkg binary packages (SDL3, SQLite), one entry per runner
  image version.
- All hosts: an [sccache](https://github.com/mozilla/sccache) compiler cache
  (pinned release, SHA-256 checked). Each run restores the newest entry for
  its OS and saves a new one; the `prune-caches` job deletes the older ones.

The compiler cache contains objects compiled from the private source. A
workflow run of a pull request from a fork can read this repository's
default-branch caches (first-time contributors need approval), so the cache
is stored encrypted with the secret `CACHE_ENCRYPTION_KEY` (a random value;
such runs do not get secrets).
Without the secret the build still works, uncached. To rotate the key, set a
new random value:

```sh
openssl rand -base64 48 | gh secret set CACHE_ENCRYPTION_KEY -R crazylin/quicksa-next-build
```

An entry encrypted with the old key fails to decrypt and is ignored. A cache
miss, or a cache that cannot be used, only makes the build slower.

`mode-viewer-windows.yml` builds the Stage 4 VTK spike of the mode-shape
viewer on Windows and, unlike `build.yml`, uploads the compiled viewer
(binaries and generated sample files, no source) as an artifact with a
3-day retention. Artifacts of a public repository can be downloaded by any
signed-in GitHub user; delete the run after testing.

```sh
gh workflow run mode-viewer-windows.yml -R crazylin/quicksa-next-build \
  -f ref=stage4-vtk-shape-spike-2026-09-30
gh run download -R crazylin/quicksa-next-build -n quicksa-mode-viewer-windows-vtk
```
