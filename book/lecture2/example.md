## Example 

Let us use what we have just learned about [element loads](./element_loads.md) and [non-zero boundary conditions](./direchlet.md) on a simple example:

```{figure} twobarqdiri.svg
:name: twobarqdiri
:align: center
```

This example has the same two-element bar model as in [](../lecture1/directly.md), so the unconstrained stiffness matrix is unchanged:

$$
\begin{bmatrix}
	\class{cA}{\cfrac{EA_1}{\ell_1}} & \class{cA}{-\cfrac{EA_1}{\ell_1}} & 0\\
	\class{cA}{-\cfrac{EA_1}{\ell_1}} & \class{cA}{\cfrac{EA_1}{\ell_1}} + \class{cB}{\cfrac{EA_2}{\ell_2}} & \class{cB}{-\cfrac{EA_2}{\ell_2}}\\
	0 & \class{cB}{-\cfrac{EA_2}{\ell_2}} & \class{cB}{\cfrac{EA_2}{\ell_2}}\\
      \end{bmatrix}
$$

Both supports will introduce a support reaction:

```{figure} assembly5_3.svg
:name: assembly5_3
:align: center
```

$$
      \begin{bmatrix}
	\class{cA}{\cfrac{EA_1}{\ell_1}} & \class{cA}{-\cfrac{EA_1}{\ell_1}} & 0\\
	\class{cA}{-\cfrac{EA_1}{\ell_1}} & \class{cA}{\cfrac{EA_1}{\ell_1}} + \class{cB}{\cfrac{EA_2}{\ell_2}} & \class{cB}{-\cfrac{EA_2}{\ell_2}}\\
	0 & \class{cB}{-\cfrac{EA_2}{\ell_2}} & \class{cB}{\cfrac{EA_2}{\ell_2}}\\
      \end{bmatrix}
      \begin{bmatrix}
	u_1\\[12pt]u_2\\[12pt]u_3
      \end{bmatrix}
      =
      \begin{bmatrix}
	H_1\\[12pt]0\\[12pt]H_3
      \end{bmatrix}
      $$

## Distributed load

The distributed load can be added directly to the global force vector as the local coordinate system aligns with the global coordinate system