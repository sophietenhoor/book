# Recap differential equations for structures

Let's recap how to solve a structure using differential equations.

Given is a cantilever beam with a distributed load:

```{figure} bending1field.svg
:name: bending1field
:align: center

Cantilever beam with distributed load
```

The differential equation for the Euler-Bernoulli model can be derived leading to:
- Kinematic relations:
  - $\varphi=-\frac{\mrm{d}w}{\mrm{d}x}$
  - $\kappa=\frac{\mrm{d}\varphi}$
- Constitutive relation:
  - $M=EI\kappa$
- Equilibrium relations:
  - $\frac{\mrm{d}V}{\mrm{d}x}=-q$
  - $\frac{\mrm{d}M}{\mrm{d}x}=V$