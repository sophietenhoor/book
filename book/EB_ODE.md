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

# Solving Euler Bernoulli ODE with SymPy

```{code-cell} ipython3
import sympy as sym
sym.init_printing()
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
display(C_sol)
```

```{code-cell} ipython3
w.subs(C_sol)
```
