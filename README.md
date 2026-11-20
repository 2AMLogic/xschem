# 2AMLogic/xschem

A fork of [StefanSchippers/xschem](https://github.com/StefanSchippers/xschem),
the schematic editor and netlister. It does two things:

1. **Publishes prebuilt xschem binaries.** Each release
   (`<upstream tag>-2am.<build>`, for example `3.4.7-2am.1`) is an unmodified
   build of an upstream tag for Ubuntu 24.04 x86_64. Its git tag points at the
   upstream commit, so the release's source archives are the exact source the
   binary was built from. Upstream's `LICENSE` is attached to every release.
   The build prefix is `/opt/eda/xschem` and the tree is **not relocatable**:
   xschem compiles its symbol library path in, so unpack it at that prefix.
   Chip CI installs these binaries without credentials through
   2AMLogic/klayout-tools `scripts/install-xschem.sh`, which verifies the
   sha256. The fleet's xschem version of record is
   2AMLogic/klayout-tools `docker/eda-sim/pdk-versions.json` `tools.xschem`
   (fleet rule 2AMLogic/2am#7275).
2. **Holds upstream bug work.** Fixes for xschem bugs we find are prepared on
   branches here and offered upstream as pull requests.

This branch (`2am-release`, the default) holds only this README and the
release workflow. Upstream's branches (`master` and the rest) and tags are
kept as upstream has them.

## Cutting a release

1. Sync upstream: `gh repo sync 2AMLogic/xschem --branch master`, then push
   the new upstream tag to this fork if it is missing.
2. Dispatch **Build and publish an xschem release** (`.github/workflows/release.yml`)
   with the upstream `tag` and a `build` number (start at 1; a rebuild of
   the same tag takes the next number).
3. Record the values from the run summary in klayout-tools'
   `pdk-versions.json` by PR.

This repository is not in the 2AMLogic agent fleet (`fleet: false` in the
fleet store); changes here are made by attended sessions.
