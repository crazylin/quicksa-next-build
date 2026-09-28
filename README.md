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

Run logs of a public repository are public. No build artifacts are uploaded.
