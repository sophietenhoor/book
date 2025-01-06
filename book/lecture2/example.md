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

Both supports will introduce a support reaction in the form of a Neumann boundary condition:

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

The distributed load can be added directly as equivalent load vector ${\mathbf{f}_\mathrm{eq}}$ to the global force vector as the local coordinate system aligns with the global coordinate system. Doing so for both elements leads to:

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
	H_1 \class{cA}{ + \cfrac{q\ell_1}{2}}\\ \class{cA}{\cfrac{q\ell_1}{2}} \class{cB}{+ \cfrac{q\ell_2}{2}}\\H_3 \class{cB}{ + \cfrac{q\ell_2}{2}}
      \end{bmatrix}
      $$

## Dirichlet boundary conditions

Let's apply the Dirichlet boundary conditions in the two different methods:

### Static condensation

Our constrained degrees of freedom are $u_1$ and $u_3$, while $u_2$ is free:

- $ \mathbf{u}_\mrm{f} = u_2$
- $ \mathbf{u}_\mrm{c} = \begin{bmatrix} u_1\\u_3\end{bmatrix}$

This gives:
- $ \mathbf{f}_\mrm{f} = \class{cA}{\cfrac{q\ell_1}{2}} \class{cB}{+ \cfrac{q\ell_2}{2}}$
- $ \mathbf{f}_\mrm{c} = \begin{bmatrix} H_1 \class{cA}{ + \cfrac{q\ell_1}{2}}\\H_3 \class{cB}{ + \cfrac{q\ell_2}{2}}\end{bmatrix}$

Partitioning the stiffness matrix leads to:

$$
\left[
\begin{array}{c|c}
\class{cA}{\cfrac{EA_1}{\ell_1}} + \class{cB}{\cfrac{EA_2}{\ell_2}} &  \class{cA}{-\cfrac{EA_1}{\ell_1}}  & \class{cB}{-\cfrac{EA_2}{\ell_2}}\\
\hline
\class{cA}{-\cfrac{EA_1}{\ell_1}} & \class{cA}{\cfrac{EA_1}{\ell_1}} & 0\\
\class{cB}{-\cfrac{EA_2}{\ell_2}} & 0 & \class{cB}{\cfrac{EA_2}{\ell_2}}\\
\end{array}
\right]
\begin{bmatrix}
u_2\\[12pt] \hline \\ 0 \\[12pt] \bar u
\end{bmatrix}
=
\begin{bmatrix}
 \class{cA}{\cfrac{q\ell_1}{2}} \class{cB}{+ \cfrac{q\ell_2}{2}} \\ 
 H_1 \class{cA}{ + \cfrac{q\ell_1}{2}}\\
 \\H_3 \class{cB}{ + \cfrac{q\ell_2}{2}}
\end{bmatrix}
$$

with:

- $\mathbf{K}_\mrm{ff} =  \class{cA}{\cfrac{EA_1}{\ell_1}} + \class{cB}{\cfrac{EA_2}{\ell_2}} $
- $\mathbf{K}_\mrm{fc} =  \begin{bmatrix} \class{cA}{-\cfrac{EA_1}{\ell_1}}  & \class{cB}{-\cfrac{EA_2}{\ell_2}} \end{bmatrix}$
- $\mathbf{K}_\mrm{cf} =  \begin{bmatrix} \class{cA}{-\cfrac{EA_1}{\ell_1}} \\ \class{cB}{-\cfrac{EA_2}{\ell_2}} \end{bmatrix}$
- $\mathbf{K}_\mrm{cc} =  \begin{bmatrix}\class{cA}{\cfrac{EA_1}{\ell_1}} & 0\\  0 & \class{cB}{\cfrac{EA_2}{\ell_2}}\\ \end{bmatrix}$
