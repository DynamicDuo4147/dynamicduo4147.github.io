---
layout: page
title: Kelvin Helmholtz Instability 
description: Kelvin Helmholtz solved with HLLC
img: assets/img/4096x4096_HLLC_Inviscid_Normalized_Density_3s.png
importance: 1
category: work
related_publications: false
---

## Introduction
The Kelvin-Helmholtz instability (KHI) is a shear-driven hydrodynamic instability observed across a wide range of flow regimes, from astrophysical phenomena to oceanic currents. In high-speed flows, KHI plays a critical role in shock--wave/boundary--layer interactions, mixing layers, jet dynamics, laminar--to--turbulent transition, and combustion processes such as those occurring in scramjet engines. In the hypersonic regime, thermal-nonequilibrium effects must be taken into account to accurately model the flow. In particular, high-fidelity approximate Riemann solvers with the numerical schemes of HLL, HLLC, and AUSM+ are implemented in OpenFOAM. Thorough validation and verification studies were performed to ensure that findings are independent of any numerical parameters. Direct numerical simulations were carried out, and the analysis reveals that AUSM+ exhibited the least numerical dissipation when compared with Kraichnan-Batchelor-Leith (KBL) theory, showing very good agreement with inviscid solutions reported in the literature.

## Case Setup
A structured rectangular domain ensuring consistency among flow domains. An open-source software, GMSH, was used for the mesh construction. The mesh consists of a three-dimensional box with dimensions $L=1$. A small extrusion is applied in the z-direction on the order of 0.01 to satisfy OpenFOAM's three-dimensional requirement. The box is divided into three layers, with the top and bottom layers assigned as low-density regions with a height of $L/4$. The middle region is the high-density region and is ascribed a height of $L/2$. The field conditions are changed based on the case environment, but the region designations remain consistent among all cases. 

<div id="fig-gridResolution">
    <div class="row">
        <div class="col-sm mt-3 mt-md-0">
            {% include figure.liquid loading="eager" path="assets/img/caseSetup.png" title="Mesh Setup" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
    <div class="caption">
        <b>Figure 1:</b> Case Setup with prescribed field conditions.
    </div>
</div>




<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/caseSetup.png" title="Mesh Setup" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/mesh.png" title="Generated Mesh" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: the case setup. Right: the generated mesh.
</div>


## Convergence

## Numerical Validation

## Results


You can also put regular text between your rows of images, even citations {% cite einstein1950meaning %}.
Say you wanted to write a bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, _bled_ for your project, and then... you reveal its glory in the next row of images.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %}
