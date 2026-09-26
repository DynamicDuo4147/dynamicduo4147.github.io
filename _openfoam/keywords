---
layout: post
title: "01 – CFD Keywords"
description: Keywords and definitions
order: 1
toc:
  sidebar: left
---

## Glossary

### Time integration

| Term | Definition |
|---|---|
| **Implicit** | Evaluates residuals at the new time level, leading to a (generally nonlinear) system of algebraic equations each step. Unconditionally stable for many model problems, so it permits larger $$\Delta t$$ at the cost of solving simultaneous systems. |
| **Explicit** | Updates unknowns using only already-known states (e.g., time level $$n$$). Conditionally stable: it must satisfy a CFL-type constraint (small $$\Delta t$$), but each step is cheap and needs no coupled system solve. |

### Transport and operators

| Term | Definition |
|---|---|
| **Flux** | Rate of flow of a conserved quantity per unit area. For a scalar $$\phi$$, the total flux is often $$\mathbf{J}_\phi = \phi\,\mathbf{u} - \Gamma \nabla \phi$$ (advective + diffusive); the flux through a face with unit normal $$\mathbf{n}$$ is $$\mathbf{J}_\phi \cdot \mathbf{n}$$. |
| **Advection** | Transport of $$\phi$$ by the bulk velocity $$\mathbf{u}$$, represented by $$\nabla \cdot (\phi\,\mathbf{u})$$ or $$\mathbf{u} \cdot \nabla \phi$$. |
| **Convection** | In CFD usage, commonly synonymous with advection. In heat/mass-transfer contexts it may mean advection plus diffusion. |
| **Diffusion** | Transport driven by gradients (Fick's, Fourier's, Newton's laws). In PDE form: $$\nabla \cdot (\Gamma \nabla \phi)$$, where $$\Gamma$$ is a mass, thermal, or momentum diffusivity. |
| **Laplacian** | $$\nabla^2 \phi = \nabla \cdot (\nabla \phi)$$. With constant $$\Gamma$$, the diffusive operator reduces to $$\Gamma \nabla^2 \phi$$. |
| **Divergence** | $$\nabla \cdot \mathbf{a}$$ measures net outflow per unit volume. In FV, its integral form ties directly to the face-flux balance via Gauss' theorem. |
| **Curl** | $$\nabla \times \mathbf{u}$$ measures local rotation (vorticity in fluids). |
| **Mag-square grad-grad** | Magnitude-squared of the Hessian: $$\mathrm{magSqr}(\nabla\nabla \phi) = \sum_{i,j} \left(\frac{\partial^2 \phi}{\partial x_i \partial x_j}\right)^2$$. Used in smoothness/shock sensors; for vectors/tensors use the Frobenius norm of the derivative tensor. |

### Discretization properties

| Term | Definition |
|---|---|
| **Numerical diffusion** | Artificial smoothing from discretization (upwinding, coarse meshes, poor alignment) that mimics diffusion, smearing gradients and damping small scales. |
| **Dispersion** | Wavenumber-dependent phase speed that spreads waves or scalar signals. Physical dispersion comes from the medium; numerical dispersion is phase error from discretization. |
| **Boundedness** | The scheme creates no new extrema (e.g., negative densities or overshoots), ensuring positivity / maximum-principle compliance. |
| **Sweby diagram** | Plot in the $$(r, \phi)$$ plane for assessing TVD flux limiters, where $$r$$ is the gradient ratio and $$\phi(r)$$ the limiter. Limiters inside the TVD region give monotone (bounded) reconstructions. |
| **Interpolation (FV)** | Reconstruction of face values from cell-centred values (upwind, linear, limited linear) to evaluate face fluxes. |
| **Extrapolation** | Estimating values outside the known stencil; less robust than interpolation and more prone to instability near boundaries. |

### Boundary conditions

| Term | Definition |
|---|---|
| **Dirichlet** | Prescribed field value at the boundary: $$\phi = \phi_b$$ on $$\partial\Omega$$. |
| **Neumann** | Prescribed normal derivative (flux) at the boundary: $$\nabla \phi \cdot \mathbf{n} = g$$ on $$\partial\Omega$$. |

### Linear algebra and solvers

| Term | Definition |
|---|---|
| **Symmetric matrix** | $$A = A^\top$$ ($$A_{ij} = A_{ji}$$). Discrete diffusion operators are often (block-)symmetric. |
| **Positive definite (SPD)** | Symmetric $$A$$ with $$\mathbf{x}^\top A \mathbf{x} > 0$$ for all nonzero $$\mathbf{x}$$. Guarantees a unique solution and enables efficient solvers such as Conjugate Gradient. |
| **Agglomeration** | Multigrid/AMG coarsening step that groups fine cells into larger aggregates to speed convergence of low-frequency error. |

### Rarefied gas dynamics

| Term | Definition |
|---|---|
| **Mean free path ($$\lambda$$)** | Average distance a molecule travels between collisions; increases as pressure decreases and governs rarefaction effects. |
| **Knudsen number ($$\mathrm{Kn}$$)** | $$\mathrm{Kn} = \lambda / L$$. Regimes: continuum ($$\mathrm{Kn} \ll 1$$), slip ($$\sim 10^{-3}$$ to $$10^{-1}$$), transitional ($$\sim 10^{-1}$$ to $$10$$), free-molecular ($$\gg 1$$). |

### Mesh

| Term | Definition |
|---|---|
| **Aspect ratio** | Ratio of a cell's largest to smallest length scale. High aspect ratios can degrade accuracy (anisotropic numerical diffusion) and solver conditioning. |
