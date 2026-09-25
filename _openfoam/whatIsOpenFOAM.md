---
layout: post
title: "02 – What is OpenFOAM"
description: Explaining the OpenFOAM environment and architecture.
order: 2
toc:
  sidebar: left
---

This guide focuses on explaining OpenFOAM's case directories. After compilation, a brief gander at the tutorial cases opens several directories each focusing on a specific portion of the simulation. 

## Before you start

With any open-source software finding documentation is crucial to understanding projects without dissecting code line by line. Unfortunately, for OpenFOAM there are two main versions circulating: foundations and ESI. Foundations versions are denoted as v11 or v14, but ESI versions are given the designation such as v2406.
Early versions of both foundations and ESI remained similar, but recently the two have diverged such that documentation is no longer interchangeable.

| Package(s) | Purpose |
|---|---|
| `https://cfd.direct/openfoam/documentation/` | Foundations Documentation Page |
| `https://www.openfoam.com/documentation/overview` | ESI Documentation Page |
| `https://www.tfd.chalmers.se/~hani/kurser/OS_CFD/` | Academic archive for OpenFOAM |
| `https://www.cfd-online.com/` | Academic archive for OpenFOAM 

## Step 1: Understanding case structure

CFD cases within OpenFoam are broken down into three primary directories: 0, constant, and system. Each directory is critical to the successful case run and omissions in any of the directories or its subdirectories will results in run failures. Initially, a brief overview is given for each directory before a more in-depth overview is given in their respective sections. Additionally, it is to be noted while each case requires these three directories additional directories may be needed. The 0 directory contains the initial field data for the prescribed mesh. The 0 or time files contain the mesh field data at that specific iteration or timestep. Furthermore, these directories allow for the post-processing and visualization. The constant directory prescribes the modelling behavior. This directory prescribes the models used for convection, diffusion, and thermophysical properties. Lastly, the system directory pertains to OpenFOAM's numerical behavior. The system directory will detail the order and type of spatial and temporal schemes used. Please note that while each directory is found within each solver's case not all 0, constant, and system directories are created equal. Difference may arise pending what solver is used.  

## Step 2: Inside the 0 directory





## Step 3: Inside the constant directory

## Step 4: Inside the system directory

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
