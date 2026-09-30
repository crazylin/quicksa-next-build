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
