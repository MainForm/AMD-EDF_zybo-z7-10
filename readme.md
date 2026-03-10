# Zybo Z7-10 Yocto Build Workspace

This repository contains a Yocto-based build workspace for the Digilent
Zybo Z7-10 board using AMD EDF as the base environment.

The workspace combines:

- `AMD_EDF/`
  The AMD Embedded Design Framework checkout managed with `repo`
- `sources/meta-zybo-z7-10/`
  A custom board support layer for Zybo Z7-10, tracked as a git submodule
- `build_zybo-z7-10/`
  The build directory configured for the Zybo Z7-10 target


## Repository Layout

```text
.
├── AMD_EDF/
├── build_zybo-z7-10/
└── sources/
    └── meta-zybo-z7-10/
```


## Initial Setup

AMD EDF is based on the Xilinx `yocto-manifests` workflow and uses the
`repo` tool to fetch the full layer stack.

Install `repo`:

```bash
curl https://storage.googleapis.com/git-repo-downloads/repo > repo
chmod a+x repo
mkdir -p ~/bin
mv repo ~/bin/
export PATH=~/bin:$PATH
repo --help
```

Sync the AMD EDF workspace:

```bash
./setupEDF.sh
```

Initialize the Zybo Z7-10 layer submodule:

```bash
git submodule update --init --recursive
```


## Build Environment

Set up the Yocto build environment:

```bash
cd AMD_EDF
source setupsdk ../build_zybo-z7-10
```

The build is configured to use:

- `MACHINE = "zybo-z7-10"`
- the local `meta-zybo-z7-10` layer
- a local `system.xsa` through `external-hdf`
- a Digilent-based kernel recipe


## Build

After sourcing the environment, build an image with BitBake as usual. For
example:

```bash
bitbake petalinux-image-minimal
```

Use `bitbake -e virtual/kernel` or `bitbake -e device-tree` to confirm the
selected kernel and device tree configuration when debugging board bring-up.


## Notes

- `meta-zybo-z7-10` contains the board-specific machine, kernel, and device
  tree customizations.
- `build_zybo-z7-10/conf/` contains the active local build configuration.
- The AMD EDF workspace may generate `.repo/` metadata; this is workspace
  state, not board support metadata.
