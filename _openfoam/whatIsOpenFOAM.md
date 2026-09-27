---
layout: post
title: "02 – OpenFOAM Architecture"
description: Explaining the OpenFOAM environment and architecture.
order: 2
toc:
  sidebar: left
---

This guide focuses on explaining OpenFOAM's case directories. After compilation, a brief gander at the tutorial cases opens several directories each focusing on a specific portion of the simulation. Lastly, this is for OpenFOAM ESI not OpenFOAM Foundations.

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

CFD cases within OpenFOAM are broken down into three primary directories: 0, constant, and system. Each directory is critical to a successful case run, and omissions in any of the directories or their subdirectories will result in run failures. Initially, a brief overview is given for each directory before a more in-depth overview is given in their respective sections. Additionally, it is to be noted that while each case requires these three directories, additional directories may be needed. The 0 directory contains the initial field data for the prescribed mesh. The 0 or time files contain the mesh field data at that specific iteration or timestep. Furthermore, these directories allow for post-processing and visualization. The constant directory prescribes the physical model and properties. Lastly, the system directory pertains to OpenFOAM's numerical behavior. The system directory will detail the order and type of spatial and temporal schemes used. Please note that while each directory is found within each solver's case, not all 0, constant, and system directories are created equal. Differences may arise depending on which solver is used. 

```
cavity/
├── 0/                      initial & boundary conditions
│   ├── p
│   └── U
├── constant/               physical properties & mesh
│   ├── polyMesh/           (written by blockMesh)
│   └── transportProperties
└── system/                 numerics & run control
    ├── blockMeshDict
    ├── controlDict
    ├── fvSchemes
    └── fvSolution
```

## Step 2: Inside the 0 directory

The 0 directory houses the case's initial field data (e.g., pressure, velocity, etc.). The 0 directory hosts various field files with nomenclature following known abbreviations for the field (the velocity field file is called U). Each field file is comprised of four main portions: FoamFile, dimensions, internalField, and boundaryField. FoamFile specifies the data version, field type (scalar vs. vector), and field object. While learning OpenFOAM, this header can be overlooked until later, but it is to be noted that when creating a new field file, the class and object variables must match the field you are trying to create. The dimensions array within OpenFOAM assigns dimensions to the file. Each array index denotes not only the unit but also the power of that unit. Starting from left to right, the indices are as follows.

| Index No. | Property | SI Unit |
|---|---|---|
| `1` | Mass | kilogram (kg) | 
| `2` | Length | meter (m) | 
| `3` | Time | second (s) | 
| `4` | Temperature | Kelvin (K) | 
| `5` | Quanity | mole (mol) | 
| `6` | Current | ampere (A) | 
| `7` | Luminous intensity | candela (cd) |

For example, velocity (m/s) is [0 1 -1 0 0 0 0]. 

> **You cannot just interchange field files** 
> You cannot just interchange field files: If you accidentally delete a 0 directory file, you cannot copy and rename another 0 directory file without changing the dimensions array. Also, make sure your class variable matches the field (e.g., velocity has the class volVectorField). 
{: .block-warning }

internalField and boundaryField allow the user to assign values to the simulation. internalField assigns all mesh points a blanket value, while boundaryField assigns specific values to specific boundary patches within the mesh. Boundary types and values will be covered in another section.

> **Further reading material on boundary conditions can be found here:**
>
> - [CFD Monkey: A brief explanation of boundary conditions in OpenFOAM](https://cfdmonkey.com/a-brief-explanation-of-boundary-conditions-in-openfoam/)
> - [OpenFOAM User Guide: 4.2 Boundaries](https://www.openfoam.com/documentation/user-guide/4-mesh-generation-and-conversion/4.2-boundaries)
{: .block-tip }
 

## Step 3: Inside the constant directory
 
While the 0 directory handles the fluid condition, the constant directory specifies how the fluid is modelled. The constant directory has three primary files: polyMesh, turbulenceProperties, and thermophysicalProperties. polyMesh is where the mesh is stored. turbulenceProperties specifies the turbulence model, such as RANS, used during the run. Foundation OpenFOAM is packaged with the following turbulence models:
 
### Simulation type
 
| Turbulence Model | Purpose |
|---|---|
| `laminar` | No turbulence model |
| `RAS` | Reynolds-averaged stress modelling |
| `LES` | Large-eddy simulation |
 
### RAS models (common examples)
 
| Turbulence Model | Purpose |
|---|---|
| `kEpsilon` | Standard k-ε model |
| `realizableKE` | Realizable k-ε model |
| `kOmegaSST` | k-ω SST model |
| `SpalartAllmaras` | Spalart-Allmaras one-equation model for external flows |
 
### LES models (common examples, including hybrid DES)
 
| Turbulence Model | Purpose |
|---|---|
| `Smagorinsky` | Smagorinsky SGS model |
| `WALE` | Wall-adapting local eddy-viscosity SGS model |
| `kEqn` | One-equation eddy-viscosity model |
| `SpalartAllmarasDDES` | Spalart-Allmaras delayed DES (hybrid RANS-LES) |
| `kOmegaSSTDES` | k-ω SST DES (hybrid RANS-LES) |
 
> **WARNING:**
> Your OpenFOAM distribution and version dictate which turbulence models you have available. The list provided was for OpenFOAM Foundation v12.
> Additionally, each turbulence model may require specific field variables.
{: .block-warning }
 
The thermophysicalModel dictates the relationship between enthalpy, pressure, and temperature. The thermophysicalModels are called out as shown below.

```cpp
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

OpenFOAM does not assume anything; you must become acquainted with the models and equations deployed above to ensure accurate modelling. Below, a table has been created from the OpenFOAM ESI documentation.
 
### Equation of state: `equationOfState`

| Model | Purpose |
|---|---|
| `icoPolynomial` | Incompressible polynomial equation of state, e.g. for liquids |
| `perfectGas` | Perfect gas equation of state |

### Basic thermophysical properties: `thermo`
 
| Model | Purpose |
|---|---|
| `eConstThermo` | Constant specific heat $$c_p$$, with evaluation of internal energy $$e$$ and entropy $$s$$ |
| `hConstThermo` | Constant specific heat $$c_p$$, with evaluation of enthalpy $$h$$ and entropy $$s$$ |
| `hPolynomialThermo` | $$c_p$$ evaluated from polynomial coefficients, from which $$h$$ and $$s$$ are evaluated |
| `janafThermo` | $$c_p$$ evaluated from JANAF thermodynamic table coefficients, from which $$h$$ and $$s$$ are evaluated |
 
### Derived thermophysical properties: `specie`
 
| Model | Purpose |
|---|---|
| `specieThermo` | Thermophysical properties of species, derived from $$c_p$$, $$h$$ and/or $$s$$ |
 
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
| `homogeneousMixture` | Combustion mixture based on normalised fuel mass fraction $$b$$ |
| `inhomogeneousMixture` | Combustion mixture based on $$b$$ and total fuel mass fraction $$f_t$$ |
| `veryInhomogeneousMixture` | Combustion mixture based on $$b$$, $$f_t$$ and unburnt fuel mass fraction $$f_u$$ |
| `dieselMixture` | Combustion mixture based on $$f_t$$ and $$f_u$$ |
| `basicMultiComponentMixture` | Basic mixture based on multiple components |
| `multiComponentMixture` | Derived mixture based on multiple components |
| `reactingMixture` | Combustion mixture using thermodynamics and reaction schemes |
| `egrMixture` | Exhaust gas recirculation mixture |
 
### Thermophysical model: `type`
 
| Model | Purpose |
|---|---|
| `hePsiThermo` | General calculation based on enthalpy $$h$$ or internal energy $$e$$, and compressibility $$\psi$$ |
| `heRhoThermo` | General calculation based on $$h$$ or $$e$$, and density $$\rho$$ |
| `hePsiMixtureThermo` | Enthalpy for a combustion mixture based on $$h$$ or $$e$$, and $$\psi$$ |
| `heRhoMixtureThermo` | Enthalpy for a combustion mixture based on $$h$$ or $$e$$, and $$\rho$$ |
| `heheuMixtureThermo` | $$h$$ or $$e$$ for the unburnt ($$u$$) gas and the combustion mixture |
 
> **Error from thermophysical model:**
> If you receive an error from a bad selection in the thermophysical table, OpenFOAM will terminate and show you the list of accepted inputs.
{: .block-tip }

## Step 4: Inside the system directory

The system directory specifies the numerical methods used within the simulation. A brief overview of the system directory's files is shown below.
 
| File | Purpose |
|---|---|
| `controlDict` | Handles general runtime guidelines such as writeIntervals and timesteps |
| `fvSchemes` | fvSchemes handles the flux limiters to ensure convergence |
| `fvSolution` | fvSolution handles how OpenFOAM solves the system of equations |
| `blockMeshDict` | blockMeshDict details boundary patches and general mesh data |
| `misc` | Additional simulation functions (e.g., decomposeParDict) are usually found here |

While being introduced to OpenFOAM, some of the nuances will be discussed later, primarily fvSolution and fvSchemes. However, the controlDict file is almost the case conductor, wherein it controls the timesteps, solver used, and output format. An example controlDict has been provided below. Comments or explanations are provided through C++-style comments, `//`.
 
```cpp
FoamFile
{
    version     2.0;
    format      ascii;
    class       dictionary;
    location    "system";
    object      controlDict;
}
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //
 
// Defines the solver used
application     simpleFoam;
 
// Start time
startFrom       startTime;
 
startTime       0;
// End time
stopAt          endTime;
 
endTime         1000;
// Change in time
deltaT          1;
// When OpenFOAM chooses to write the files
writeControl    timeStep;
// Writes every 10 timesteps
writeInterval   10;
// Limits the number of written timesteps
purgeWrite      10;
// Output format
writeFormat     ascii;
// Written output precision
writePrecision  6;
 
writeCompression off;
 
timeFormat      general;
// Number of digits in the time directory names
timePrecision   6;
// Cases are re-read, allowing changes to schemes or endTime mid-run
runTimeModifiable true;
 
// Additional functions added for the simulation
functions
{
    #include "readFields"
    #include "streamLines"
}
```
 
> **Further reading:**
> If you would like further reading on the nuances of limiters, etc., [Holzmann CFD](https://holzmann-cfd.com/) is a great resource.
{: .block-tip }
 
## Closing
 
This has served as a crash course in the OpenFOAM file system. More information than needed has been provided to you so you can use this as a reference later on. Don't be discouraged; Rome wasn't built in a day!
