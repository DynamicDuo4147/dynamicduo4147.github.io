---
layout: page
title: Kelvin Helmholtz Instability 
description: Graduate Thesis 
img: assets/img/4096x4096_HLLC_Inviscid_Normalized_Density_3s.png
importance: 1
category: work
related_publications: false
---

## Introduction
The Kelvin-Helmholtz instability (KHI) is a shear-driven hydrodynamic instability observed across a wide range of flow regimes, from astrophysical phenomena to oceanic currents. In high-speed flows, KHI plays a critical role in shock--wave/boundary--layer interactions, mixing layers, jet dynamics, laminar--to--turbulent transition, and combustion processes. While the Kelvin Helmholtz Instability has been studied in the each regime none has studied the evolution of the Kelvin Helmholtz Instability as well as controlling for the numerical biases found within the numerical schemes. It was my hope that in undertaking this project to use OpenFOAM to evaluate the Kelvin Helmholtz Instability within an ideal framework to establish (i) OpenFOAM's and finite volume method (FVM) ability or inability to model the KHI (ii) the potential gains or losses of using approximate Riemann schemes. OpenFOAM's $rhoCentralFoam$ was modified replacing the default Kurganov and Tadmor central scheme with Harten, Lax, and van Leer (HLL and HLLC), and Advection Upstream Splitting Method (AUSM). 




from an ideal framework, inviscid and subsonic, to the hypersonic regime. To accomplish such a lofty goal a framework was built upon from Omer San within OpenFOAM using a modified $rhoCentralFoam$ adding numerical schemes: AUSM+, HLL, and HLLC. 

Thorough validation and verification studies were performed to ensure that findings are independent of any numerical parameters. Direct numerical simulations were carried out, and the analysis reveals that AUSM+ exhibited the least numerical dissipation when compared with Kraichnan-Batchelor-Leith (KBL) theory, showing very good agreement with inviscid solutions reported in the literature.

## Case Setup
A structured rectangular domain ensuring consistency among flow domains. An open-source software, GMSH, was used for the mesh construction. The mesh consists of a three-dimensional box with dimensions $L=1$. A small extrusion is applied in the z-direction on the order of 0.01 to satisfy OpenFOAM's three-dimensional requirement. The box dimensions are taken from San et al. {% cite san2015riemann --file references %}. The box is divided into three layers, with the top and bottom layers assigned as low-density regions with a height of $L/4$. The middle region is the high-density region and is ascribed a height of $L/2$. The field conditions are changed based on the case environment, but the region designations remain consistent among all cases. 

<div id="fig-gridResolution">
    <div class="row">
        <div class="col-sm mt-4 mt-md-0">
            {% include figure.liquid loading="eager" path="assets/img/caseSetup.png" title="Mesh Setup" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
    <div class="caption">
        <b>Figure 1:</b> Field conditions for the nondimensional case setup.
    </div>
</div>

Cyclic boundary conditions were imposed in every cardinal direction allowing for the simulation of an infinite domain. The front and back faces were assigned 'empty' to reduce dimensionality to remain computationally viable.

## Convergence

As this numerical model was constructed from scratch, special attention was paid to the sensitivity to numerical parameters. In particular, different grid resolutions were tested due to the turbulent nature of the problem. Therefore, turbulence was not modeled but fully resolved by capturing all relevant scales of motion. To achieve this, the grid size was chosen to be comparable to the Kolmogorov scale, which will be discussed in subsequent chapters. 

The previous investigation revealed that numerical dependence can have a significant impact on the evolution of the Kelvin--Helmholtz instability, necessitating a detailed investigation using the native Kurganov--Tadmor scheme. Meshes were constructed with nearly uniform node distributions in both the $$x$$ and $$y$$ directions, while varying the total node count to assess resolution effects. The previously described non-dimensional inviscid case was simulated at resolutions of $$1024\times1024$$, $$2048\times2048$$, $$4096\times4096$$, and $$6144\times6144$$. To characterize the impact of grid resolution on the flow field, the axial mean velocity, $$\bar{u}$$, was evaluated at each resolution for times $$t^* = 1.0$$ and $$t^* = 3.0$$. As shown in <a class="figref" href="#fig-gridResolution">Figure</a>, noticeable deviations persist for the $$1024\times1024$$ and $$2048\times2048$$ cases, whereas the $$4096\times4096$$ and $$6144\times6144$$ cases exhibit good agreement across all intervals. This demonstrates that $$\bar{u}$$ converges satisfactorily with increasing resolution, confirming that numerical independence is achieved at $$4096\times4096$$.


<div id="fig-convergence">
    <div class="row">
        <div class="col-sm mt-4 mt-md-0">
            {% include figure.liquid loading="eager" path="assets/img/Kurganov_Grid_Convergence_1s.png" title="Mesh Setup" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
    <div class="caption">
        <b>Figure 2:</b> Mean X-Velocity over the Y-Axis.
    </div>
</div>


## Numerical Validation




## Results

<div style="display: flex; flex-wrap: wrap; gap: 1rem;">
  <div style="flex: 1 1 180px;">
    {% include figure.liquid loading="eager" path="assets/img/4096x4096_Kurganov_Inviscid_Normalized_Density_1s.png" title="Image 1" class="img-fluid rounded z-depth-1" %}
  </div>
  <div style="flex: 1 1 180px;">
    {% include figure.liquid loading="eager" path="assets/img/4096x4096_HLL_Inviscid_Normalized_Density_1s.png" title="Image 2" class="img-fluid rounded z-depth-1" %}
  </div>
  <div style="flex: 1 1 180px;">
    {% include figure.liquid loading="eager" path="assets/img/4096x4096_HLLC_Inviscid_Normalized_Density_3s.png" title="Image 3" class="img-fluid rounded z-depth-1" %}
  </div>
  <div style="flex: 1 1 180px;">
    {% include figure.liquid loading="eager" path="assets/img/4096x4096_AUSM+_Inviscid_Normalized_Density_1s.png" title="Image 4" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  (a) Kurganov-Tadmor (b) HLL (c) HLLC (d) AUSM+
</div>

<div class="caption" id="table-compCost">
  <b>Table 1:</b> Comparison of the computational cost among the numerical schemes.
</div>

| **Numerical Scheme** | **Run Duration (hr)** | **CPU Hours** |
|:---:|:---:|:---:|
| Kurganov–Tadmor | 3.96 | 1518.85 |
| HLL | 5.88 | 2258.21 |
| HLLC | 6.15 | 2359.89 |
| AUSM+ | 5.83 | 2237.48 |


## References
@article{san2015riemann,
  title   = {Evaluation of Riemann flux solvers for WENO reconstruction schemes: Kelvin--Helmholtz instability},
  author  = {San, Omer and Kara, Kursat},
  journal = {Computers \& Fluids},
  volume  = {117},
  pages   = {24--41},
  year    = {2015}
}
