# Element loads

As the matrix method is a discrete approach, nodal loads were treated with ease. However, what to do with continuous loads or loads which are not applied at the nodes?

::::::{topic} Learning objective
You'll look into how to model element loads using differential equations and conservation of work and how these are combined in the matrix formulation.
::::::

## Force-displacement relations using differential equations
As [before](../lecture1/single_element.md), we can derive the force-displacement relations of a single extension element. However, now let's include the loads, for example a continuous load $q$

```{figure} extensionelementq.svg
:name: extensionelementq
:align: center

Single extension element with distributed load $q$
```

The same approach is used as in [](../lecture1/recap.ipynb). This results in:

- $C_1 = \class{cA}{\cfrac{q\ell}{2EA}}+\cfrac{u_2-u_1}{\ell}$
- $C_2 = u_1$

The continuous distributions for the displacement and section force can be evaluated too:
- $u(x) = \class{cA}{\cfrac{q}{2EA}\left(\ell x - x^2\right)} + u_1\left(1-\cfrac{x}{\ell}\right) + u_2\cfrac{x}{\ell}$
- $N(x) = \class{cA}{\cfrac{q}{2}\left(\ell-2x \right)} -\cfrac{EA}{\ell}u_1+\cfrac{EA}{\ell}u_2$

These are extended results in comparison to [before](../lecture1/single_element.md)

## Combine elements
As before, we can glue elements together by applying nodal equilibrium:

This leads to:
- $ F_1 = - N = \cfrac{EA}{\ell}u_1-\cfrac{EA}{\ell}u_2 \class{cA}{ -\cfrac{q\ell}{2}}$
- $ F_2 = N = -\cfrac{EA}{\ell}u_1+\cfrac{EA}{\ell} u_2 \class{cA}{-\cfrac{q\ell}{2}}$

or in matrix notation:

$$\cfrac{EA}{\ell}\begin{bmatrix} 1&-1\\-1&1 \end{bmatrix}\begin{bmatrix}u_1\\u_2\end{bmatrix} \class{cA}{- \begin{bmatrix} \cfrac{q\ell}{2}\\ \cfrac{q\ell}{2}\end{bmatrix}}= \begin{bmatrix}F_1\\F_2\end{bmatrix}$$

Effectively, we converted the continuous load to an equivalent nodal load.

This influences the nodal equilibrium (in the global coordinate system) too:

$$\begin{align} -\sum_e\mathbf{f}^e + \mathbf{f}_\text{nodal}& = \mathbf{0}\\
-\sum_e\left(\mathbf{K}^e\mathbf{u}^e \class{cA}{-\mathbf{f}_\mathrm{eq}^e}\right) + \mathbf{f}_\mathrm{nodal}& = \mathbf{0}\\
\sum_e\mathbf{f}^e& = \mathbf{f}_\text{nodal} + \class{cA}{\sum_e\mathbf{f}_\mathrm{eq}^e} \end{align}$$

This means that we can calculate all the equivalent nodal loads separately and add them to the nodal loads. Please note that all these forces should be in the global coordinate system: $\class{cA}{\mathbf{f}_\mathrm{eq}} = \mathbf{T}^\mathrm{T}\class{cB}{\bar{\mathbf{f}}_\mathrm{eq}}$

## Force-displacement relations using conservation of work
Let's consider another example in which the application of the differential equations are not trivial: we'll introduce a discontinuous force in the form of a point load halfway the element. We'll compare this with the same element loaded by vertical forces and bending moments at the ends:

```{figure} eqwork3.svg
:name: eqwork3
:align: center

Single extension element with point load $P$ halfway in comparison to element loaded by vertical forces and bending moments at the ends 
```

The conservation of work states that the work done by the force $P$ should be equal to the force done by the forces $\class{cA}{F_1^{eq}}$, $\class{cE}{F_2^{eq}}$, $\class{cB}{T_1^{eq}}$ and $\class{cI}{T_2^{eq}}$: $W_P$ = $W_{eq}$

To ease the calculation, we'll split the displacement in four cubic shape functions. Each shape function will have a displacement of $1$ in direction of each of the four edge forces ($\class{cA}{w_1}$, $\class{cE}{\varphi_1}$, $\class{cB}{w_2}$ and $\class{cI}{\varphi_2}$):

```{figure} shape_functions.svg
:name: shape_functions
:align: center

Four shape functions
```

Consequently, the work $W_{eq}$ can be splitted in four independent terms easing the calculation.