---
layout: post
title: "03 – Installing OpenFOAM v2406 on Ubuntu 24.04"
description: Dependencies, environment setup, and compiling OpenFOAM from source
order: 3
toc:
  sidebar: left
---

This guide covers building **OpenFOAM v2406** (ESI release) from source on
**Ubuntu 24.04**. Building from source, rather than installing a precompiled package,
is what you want if you plan to compile additional solvers such as hy2Foam.

> **Tested with:** OpenFOAM v2406 · Ubuntu 24.04 LTS
{: .block-tip }

## Before you start

Dependencies differ between OpenFOAM versions and between the solvers you build on top
of it. **Cross-reference the requirements of the specific solver you plan to compile**
before installing anything.

> The list below is what worked for me on this exact version and OS. Other
> combinations may need different package versions.
{: .block-warning }

## Step 1: Install dependencies

Copy and paste the whole block into a terminal:

```bash
sudo apt-get update
sudo apt-get install -y \
  build-essential cmake \
  openmpi-bin libopenmpi-dev \
  flex bison \
  zlib1g-dev \
  qtbase5-dev libxt-dev \
  libscotch-dev libparmetis-dev libscotchmetis-dev \
  libboost-system-dev libboost-filesystem-dev \
  libgmp-dev libmpfr-dev \
  python3 python3-dev python3-numpy
```

What each group is for:

| Package(s) | Purpose |
|---|---|
| `build-essential` | GCC/G++ compilers and `make` |
| `cmake` | Builds the third-party libraries |
| `openmpi-bin`, `libopenmpi-dev` | MPI for running cases in parallel |
| `flex`, `bison` | Lexer/parser generators OpenFOAM uses to read dictionaries |
| `zlib1g-dev` | Compression library |
| `qtbase5-dev`, `libxt-dev` | Qt and X11 libraries for GUI utilities |
| `libscotch-dev`, `libparmetis-dev`, `libscotchmetis-dev` | Mesh decomposition for parallel runs |
| `libboost-system-dev`, `libboost-filesystem-dev` | Boost libraries used by some utilities |
| `libgmp-dev`, `libmpfr-dev` | High-precision arithmetic (optional) |
| `python3`, `python3-dev`, `python3-numpy` | Python-based utilities and post-processing |

> **Why the comments aren't inside the command:** in bash, a line-continuation
> backslash must be the *very last character* on the line. Putting a `# comment`
> after it silently ends the command early, so only part of the list gets installed.
{: .block-tip }

## Step 2: Download the source

```bash
mkdir -p ~/openfoam && cd ~/openfoam
wget https://dl.openfoam.com/source/v2406/OpenFOAM-v2406.tgz
wget https://dl.openfoam.com/source/v2406/ThirdParty-v2406.tgz
tar -xzf OpenFOAM-v2406.tgz
tar -xzf ThirdParty-v2406.tgz
```

This gives you two folders side by side: `OpenFOAM-v2406` (the core code) and
`ThirdParty-v2406` (external libraries OpenFOAM compiles alongside itself).

## Step 3: Source the environment

OpenFOAM relies on environment variables (`$WM_PROJECT_DIR`, `$FOAM_TUTORIALS`,
`$FOAM_RUN`, and others) that are set by its `bashrc` file:

```bash
source ~/openfoam/OpenFOAM-v2406/etc/bashrc
```

To avoid typing this in every new terminal, add an alias to your `~/.bashrc`:

```bash
echo "alias of2406='source ~/openfoam/OpenFOAM-v2406/etc/bashrc'" >> ~/.bashrc
```

After reopening the terminal, typing `of2406` loads the environment. An alias is
safer than sourcing automatically, since it lets you switch between OpenFOAM versions
later without conflicts.

## Step 4: Compile

```bash
cd $WM_PROJECT_DIR
./Allwmake -j -s -q -l
```

| Flag | Meaning |
|---|---|
| `-j` | Compile in parallel using all available cores |
| `-s` | Silent mode (less terminal output) |
| `-q` | Queue mode (more efficient ordering of the build) |
| `-l` | Write a log file (`log.linux64...`) in the project directory |

> Compiling takes anywhere from under an hour to several hours depending on your
> CPU. If the build fails, search the log file for the first `Error`. The first
> error is almost always the real one; later errors cascade from it.
{: .block-warning }

## Step 5: Verify the installation

Check that the environment and the main executables are in place:

```bash
foamInstallationTest
NOTE: This does not work on HPC clusters. 
```

Then run the classic lid-driven cavity case as a smoke test:

```bash
mkdir -p $FOAM_RUN
cp -r $FOAM_TUTORIALS/incompressible/icoFoam/cavity/cavity $FOAM_RUN
cd $FOAM_RUN/cavity
blockMesh
icoFoam
```

If `icoFoam` runs to completion and writes time directories (`0.1`, `0.2`, ...), your
installation works.

## Troubleshooting

- **`E: Package 'qt5-default' has no installation candidate`**: `qt5-default` was
  removed after Ubuntu 20.04. Use `qtbase5-dev` instead, as in the list above.
- **`command not found` for OpenFOAM tools**: the environment isn't loaded in this
  terminal. Run `of2406` (or the full `source` command) first.
- **A package isn't found by `apt`**: check whether it was renamed for your Ubuntu
  version with `apt search <name>`, and compare against `Requirements.md`.
