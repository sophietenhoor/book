---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.6
kernelspec:
  display_name: base
  language: python
  name: python3
---

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
  - $\varphi=-\cfrac{\mrm{d}w}{\mrm{d}x}$
  - $\kappa=\cfrac{\mrm{d}\varphi}{\mrm{d}x}$
- Constitutive relation:
  - $M=EI\kappa$
- Equilibrium relations:
  - $\cfrac{\mrm{d}V}{\mrm{d}x}=-q$
  - $\cfrac{\mrm{d}M}{\mrm{d}x}=V$

These relations can be combined into one fourth order differential equation:

$$ EI\frac{\mrm{d}^4w}{\mrm{d}x^4}=q $$

+++

This differential equation can be solved directly to solve structures.

+++

## Solving differential equation of one field by hand

This differential equations can be solved by integrating four times:

$$w(x) = \frac{qx^4}{24EI} + \frac{C_1x^3}{6} + \frac{C_2x^2}{2}+C_3x+C_4 $$

The boundary conditions follow from the clamped side at $x=0$ and free end at $x=\ell$:

- $w(0) = 0$
- $\varphi(0) = 0$
- $	M(\ell) = 0$
- $	V(\ell) = 0$

Solving these four equations for the integration constants gives:

- $C_1 = -\cfrac{q\ell}{EI}$
- $C_2 = \cfrac{q\ell^2}{2EI}$
- $C_3 = 0$
- $C_4 = 0$

Substituting these constants, a final solution for $w$ can be found:

$w(x) = \cfrac{qx^4}{24EI} - \cfrac{q\ell x^3}{6EI} + \cfrac{q\ell^2x^2}{4EI}$

+++

## Solving differential equation of one field using SymPy

The differential equation can also be solved using SymPy:

```{code-cell} ipython3
:tags: [thebe-remove-input-init]

import sympy as sym
sym.init_printing()
```

```{code-cell} ipython3
import sympy as sym
```

```{code-cell} ipython3
EI, q_z, x, L = sym.symbols('EI, q_z, x, L')
w = sym.Function('w')

ODE_bending = sym.Eq(w(x).diff(x, 4) *EI, q_z)
display(ODE_bending)
```

```{code-cell} ipython3
w = sym.dsolve(ODE_bending, w(x)).rhs
display(w)
```

```{code-cell} ipython3
phi = -w.diff(x)
kappa = phi.diff(x)
M = EI * kappa
V = M.diff(x)
eq1 = sym.Eq(w.subs(x,0),0)
eq2 = sym.Eq(phi.subs(x,0),0)
eq3 = sym.Eq(M.subs(x,L),0)
eq4 = sym.Eq(V.subs(x,L),0)
C_sol = sym.solve([eq1, eq2, eq3, eq4 ], sym.symbols('C1, C2, C3, C4'))
# display each of the constants individually as math output
for key in C_sol:
    display(sym.Eq(key, C_sol[key]))
```

```{code-cell} ipython3
w.subs(C_sol)
```

## Solving differential equation of two fields

A similar approach can be taken when solving two fields.

Let's investigate the following structure, consisting of two fields.

```{figure} extension2fields.svg
:name: extension2fields
:align: center

Extension bar with nodal load
```

As this structure is loaded along its axis, the differential equation for extension is used.

For the first field this gives:

- ${\cA\color{cA}EA_1\cfrac{\mrm{d}^2u_1}{\mrm{d}x^2}=0}$
- ${\cA\color{cA}u_1 = C_1x + C_2}$
- Boundary conditions: ${\cA\color{cA}u_1(0) = 0}$

For the second field it gives:

- ${\cB\color{cB}EA_2\cfrac{\mrm{d}^2u_2}{\mrm{d}x^2}=0}$
- ${\cB\color{cB}u_2 = C_3x + C_4}$
- Boundary conditions: ${\cB\color{cB}N_2(\ell_1+\ell_2) = F}$

The two remaining integration constants can be solved by specifying interface conditions:
- ${\cA\color{cA}u_1(\ell_1)} = {\cB\color{cB}u_2(\ell_1)}$
- ${\cA\color{cA}N_1(\ell_1)} = {\cB\color{cB}N_2(\ell_1)}$

<span style="color:blue">${EA_1\cfrac{\mrm{d}^2u_1}{\mrm{d}x^2}=0}$</span>.
