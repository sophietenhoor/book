# Element loads

As the matrix method is a discrete approach, nodal loads were treated with ease. However, what to do with continuous loads or loads which are not applied at the nodes?

::::::{topic} Learning objective
You'll look into how to model element loads using differential equations and work.
::::::

## Differential equations
As [before](../lecture1/single_element.md), we can derive the force-displacement relations of a single extension element. However, now let's include the loads, for example a continuous load $q$

```{figure} extensionelementq.svg
:name: extensionelementq
:align: center

Single extension element with distributed load $q$
```

The same approach is used as in [](./recap.ipynb). This results in:

- $C_1 = \cfrac{q\ell}{2EA}+\cfrac{u_2-u_1}{\ell}$
- $C_2 = u_1$

The continuous distributions for the displacement and section force can be evaluated too:
- $u(x) = \cfrac{q}{2EA}\left(\ell x - x^2\right) + u_1\left(1-\cfrac{x}{\ell}\right) + u_2\cfrac{x}{\ell}$
- $N(x) = \cfrac{q}{2}\left(\ell-2x \right) -\cfrac{EA}{\ell}u_1+\cfrac{EA}{\ell}u_2$

Applying nodal equilibrium gives:

This leads to:
- $ F_1 = - N = \cfrac{EA}{\ell}u_1-\cfrac{EA}{\ell}u_2 -\cfrac{q\ell}{2}$
- $ F_2 = N = -\cfrac{EA}{\ell}u_1+\cfrac{EA}{\ell} u_2 -\cfrac{q\ell}{2}$

or in matrix notation:

$$\cfrac{EA}{\ell}\begin{bmatrix} 1&-1\\-1&1 \end{bmatrix}\begin{bmatrix}u_1\\u_2\end{bmatrix} \color{cB}{- \mymat{\cfrac{q\ell}{2}\\\cfrac{q\ell}{2}}}= \begin{bmatrix}F_1\\F_2\end{bmatrix}$$