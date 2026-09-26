---
layout: post
title: "02 – OpenFOAM Architecture"
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

## Step 1: Overview of case structure

CFD cases within OpenFoam are broken down into three primary directories: 0, constant, and system. Each directory is critical to the successful case run and omissions in any of the directories or its subdirectories will results in run failures. Initially, a brief overview is given for each directory before a more in-depth overview is given in their respective sections. Additionally, it is to be noted while each case requires these three directories additional directories may be needed. The 0 directory contains the initial field data for the prescribed mesh. The 0 or time files contain the mesh field data at that specific iteration or timestep. Furthermore, these directories allow for the post-processing and visualization. The constant directory prescribes the modelling behavior. This directory prescribes the models used for convection, diffusion, and thermophysical properties. Lastly, the system directory pertains to OpenFOAM's numerical behavior. The system directory will detail the order and type of spatial and temporal schemes used. Please note that while each directory is found within each solver's case not all 0, constant, and system directories are created equal. Difference may arise pending what solver is used.  

## Step 2: Inside the 0 directory

Within the 0 directory houses the case's initial field data (i.e. pressure, velocity, etc.). The 0 directory hosts various field files with nomenclature following known abbreviations for the field (velocity field file is called U). Each field file is comprised of four main portions: FoamFile, dimensions, internalField, and boundaryField. FoamFile specifies the data version, field type (scalar v. vector), and field object. While learning OpenFOAM this header can be overlooked until later, but it is to be noted that when creating new field file that the class and object variables match field you are trying to create. The dimensions array within OpenFOAM assigns dimensions to the file. Each array index denotes not only the unit, but the power of that unit. Starting from the left to the right the indices are as follows below. 

| Index No. | Property | SI Unit | USCS Unit |
|---|---|---|---|
| `1` | Mass | kilogram (kg) | pound-mass (lbm) |
| `2` | Length | meter (m) | foot (ft) |
| `3` | time | second (s) | second (s) |
| `4` | time | Kelvin (K) | Rankine (R) |
| `5` | quanity | mole (mol) | mole (mol) |
| `6` | current | ampere (A) | ampere (A) |
| `7` | Luminous intensity | candela (cd) | candela (cd) |

For example, velocity (m/s) is [0 1 -1 0 0 0 0]. 

> **You cannot just interchange field files** 
> If you accidentally delete a 0 directory file you cannot copy and rename another 0 directory file without changing the dimensions array.
> Also, make sure your class variable matches the field (i.e. velocity has the class volVectorField). 
{: .block-warning }

internalField and boundaryField allow for the user to assign values to the simulation. internalFields assigns all mesh points a blanket value while boundaryField will assign specific boundary patches within the mesh specific values. Boundary types and values will be covered in another section. 

> **Further reading material on boundary conditions can be found here:** 
> https://cfdmonkey.com/a-brief-explanation-of-boundary-conditions-in-openfoam/ 
> https://www.openfoam.com/documentation/user-guide/4-mesh-generation-and-conversion/4.2-boundaries
{: .block-tip }

## Step 3: Inside the constant directory

While the 0 directory handles the fluid condition the constant directory specifies how the fluid is modelled. The constant directory has two primary files: turbulenceProperties and thermophysicalProperties. turbulenceProperties specify the turbulence model such as RANS, kOmegaSST, etc. used during the run. Foundations OpenFOAM is packaged with the following turbulence models:

| Turbulence Model | Purpose |
|---|---|
| `laminar` | no turbulence model |
| `RAS` | Reynolds-averaged stress modelling |
| `LES` | large-eddy simulation |

| Turbulence Model | Purpose |
|---|---|
| `LRR` | Launder, Reece and Rodi Reynolds-stress model |
| `LamBremhorstKE` | Lam and Bremhorst low-Re k-ε model |
| `LaunderSharmaKE` | Launder and Sharma low-Re k-ε model |
| `LienCubicKE` | Lien cubic non-linear low-Re k-ε model |
| `LienLeschziner` | Lien and Leschziner low-Re k-ε model |
| `RNGkEpsilon` | Renormalization group (RNG) k-ε model |
| `SSG` | Speziale, Sarkar and Gatski Reynolds-stress model |
| `ShihQuadraticKE` | Shih's quadratic algebraic Reynolds-stress k-ε model |
| `SpalartAllmaras` | Spalart-Allmaras one-equation model for external flows |
| `kEpsilon` | Standard k-ε model |
| `kEpsilonLopesdaCosta` | k-ε variant with extra source terms for porous regions (atmospheric flow over forested terrain) |
| `kOmega` | Standard high-Re k-ω model |
| `kOmega2006` | Standard (2006) high-Re k-ω model |
| `kOmegaSST` | k-ω SST model |
| `kOmegaSSTLM` | Langtry-Menter 4-equation transitional SST model |
| `kOmegaSSTSAS` | Scale-adaptive URANS model based on k-ω SST |
| `kkLOmega` | Low-Re k-kl-ω model |
| `qZeta` | Gibson and Dafa'Alla q-ζ two-equation low-Re model |
| `realizableKE` | Realizable k-ε model |
| `v2f` | Lien and Kalitzin v2-f model with Davidson et al. viscosity limit |

### RAS models (compressible)

| Turbulence Model | Purpose |
|---|---|
| `LRR` | Launder, Reece and Rodi Reynolds-stress model |
| `LaunderSharmaKE` | Launder and Sharma low-Re k-ε model, including RDT-based compression term (also for combusting flows) |
| `RNGkEpsilon` | Renormalization group (RNG) k-ε model |
| `SSG` | Speziale, Sarkar and Gatski Reynolds-stress model |
| `SpalartAllmaras` | Spalart-Allmaras one-equation model for external flows |
| `buoyantKEpsilon` | Standard k-ε with added buoyancy generation/dissipation terms |
| `kEpsilon` | Standard k-ε model, including RDT-based compression term |
| `kOmega` | Standard high-Re k-ω model |
| `kOmega2006` | Standard (2006) high-Re k-ω model |
| `kOmegaSST` | k-ω SST model |
| `kOmegaSSTLM` | Langtry-Menter 4-equation transitional SST model |
| `kOmegaSSTSAS` | Scale-adaptive URANS model based on k-ω SST |
| `realizableKE` | Realizable k-ε model |
| `v2f` | Lien and Kalitzin v2-f model with Davidson et al. viscosity limit |

### LES models (including hybrid DES)

| Turbulence Model | Purpose |
|---|---|
| `DeardorffDiffStress` | Differential SGS stress equation model |
| `Smagorinsky` | Smagorinsky SGS model |
| `WALE` | Wall-adapting local eddy-viscosity SGS model |
| `dynamicKEqn` | Dynamic one-equation eddy-viscosity model |
| `dynamicLagrangian` | Dynamic SGS model with Lagrangian averaging |
| `kEqn` | One-equation eddy-viscosity model |
| `SpalartAllmarasDES` | Spalart-Allmaras DES (hybrid RANS-LES) |
| `SpalartAllmarasDDES` | Spalart-Allmaras delayed DES |
| `SpalartAllmarasIDDES` | Spalart-Allmaras improved delayed DES |
| `kOmegaSSTDES` | k-ω SST DES |

> **WARNING** 
> Your OpenFOAM distribution and version dictates which turbulence models you have available. The list provided was for OpenFOAM Foundations v12.
> Additionally, each turbulence models may require specific field variables. 
{: .block-warning }

The thermophysicalModel dictates the relationship between enthalpy, pressure, and temperature. The thermophysicalModels are called out as shown below.  

```
thermoType
{
    type            hePsiThermo;
    mixture         pureMixture;
    transport       sutherland;
    thermo          hConst;
    equationOfState perfectGas;
    specie          specie;
    energy          sensibleInternalEnergy;
}
```

OpenFOAM does not assume anything you must become acquainted with the the models and equations deployed above to ensure accurate modelling. Below a table has been created for your viewing of the various models developed for OpenFOAM ESI. 

### Equation of state: `equationOfState`

| Model | Purpose |
|---|---|
| `icoPolynomial` | Incompressible polynomial equation of state, e.g. for liquids |
| `perfectGas` | Perfect gas equation of state |

### Basic thermophysical properties: `thermo`

| Model | Purpose |
|---|---|
| `eConstThermo` | Constant specific heat $c_p$, with evaluation of internal energy $e$ and entropy $s$ |
| `hConstThermo` | Constant specific heat $c_p$, with evaluation of enthalpy $h$ and entropy $s$ |
| `hPolynomialThermo` | $c_p$ evaluated from polynomial coefficients, from which $h$ and $s$ are evaluated |
| `janafThermo` | $c_p$ evaluated from JANAF thermodynamic table coefficients, from which $h$ and $s$ are evaluated |

### Derived thermophysical properties: `specie`

| Model | Purpose |
|---|---|
| `specieThermo` | Thermophysical properties of species, derived from $c_p$, $h$ and/or $s$ |

### Transport properties: `transport`

| Model | Purpose |
|---|---|
| `constTransport` | Constant transport properties |
| `polynomialTransport` | Polynomial-based temperature-dependent transport properties |
| `sutherlandTransport` | Sutherland's formula for temperature-dependent transport properties |

### Mixture properties: `mixture`

| Model | Purpose |
|---|---|
| `pureMixture` | General thermophysical model calculation for passive gas mixtures |
| `homogeneousMixture` | Combustion mixture based on normalised fuel mass fraction $b$ |
| `inhomogeneousMixture` | Combustion mixture based on $b$ and total fuel mass fraction $f_t$ |
| `veryInhomogeneousMixture` | Combustion mixture based on $b$, $f_t$ and unburnt fuel mass fraction $f_u$ |
| `dieselMixture` | Combustion mixture based on $f_t$ and $f_u$ |
| `basicMultiComponentMixture` | Basic mixture based on multiple components |
| `multiComponentMixture` | Derived mixture based on multiple components |
| `reactingMixture` | Combustion mixture using thermodynamics and reaction schemes |
| `egrMixture` | Exhaust gas recirculation mixture |

### Thermophysical model: `type`

| Model | Purpose |
|---|---|
| `hePsiThermo` | General calculation based on enthalpy $h$ or internal energy $e$, and compressibility $\psi$ |
| `heRhoThermo` | General calculation based on $h$ or $e$, and density $\rho$ |
| `hePsiMixtureThermo` | Enthalpy for a combustion mixture based on $h$ or $e$, and $\psi$ |
| `heRhoMixtureThermo` | Enthalpy for a combustion mixture based on $h$ or $e$, and $\rho$ |
| `heheuMixtureThermo` | $h$ or $e$ for the unburnt ($u$) gas and the combustion mixture |

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
