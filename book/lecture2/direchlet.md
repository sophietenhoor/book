# Non-zero Dirichlet boundary conditions

In [](../lecture1/directly.md) we defined how to handle Dirichlet boundary conditions which enforce a displacement of $0$ by striking the corresponding row/column in the final system. However, this approach doesn't work with nonzero displacements.

::::::{topic} Learning objective
You'll look into how to model non-zero Dirichlet boundary conditions using static condensation and a size-preserving approach
::::::

(static-condensation)=
## Static condensation
To apply nonzero constraints we can partition the system:

$$
\begin{bmatrix}
\mathbf{K}_{ff} & \mathbf{K}_{fc}\\
\mathbf{K}_{cf} & \mathbf{K}_{cc}
\end{bmatrix}
\begin{bmatrix}
\mathbf{u}_f\\
\mathbf{u}_c
\end{bmatrix}
=
\begin{bmatrix}
\mathbf{f}_f\\
\mathbf{f}_c
\end{bmatrix}
$$

With subscript $_f$ for the free degrees of freedom and the subscript $_c$ for the constraint degrees of freedom.

The unknown free displacements can now be solved for:

$$
\begin{align}
\mathbf{K}_\mrm{ff}\mathbf{u}_\mrm{f} + \mathbf{K}_\mrm{fc}\mathbf{u}_\mrm{c}& = \mathbf{f}_\mrm{f}
\\
\mathbf{u}_\mrm{f}& = \mathbf{K}_\mrm{ff}^{-1}\left(\mathbf{f}_\mrm{f} - \mathbf{K}_\mrm{fc}\mathbf{u}_\mrm{c}\right)
\end{align}
$$

Furthermore, this allows us to solve for the support reactions, which are, among the other terms from nodal and equivalent loads, part of $\mathbf{f}_\mrm{c}$:

$$
\mathbf{f}_\mrm{c} = \mathbf{K}_\mrm{cf}\mathbf{u}_\mrm{f} + \mathbf{K}_\mrm{cc}\mathbf{u}_\mrm{c}
$$

However, this approach can be annoying to code because reordering the system costs computation time and the gains when inverting the stiffness matrix are very limited.

## Size-preserving approach

An alternative approach is to modify the equations such that the system is not reordered and the size is preserved. This can be done by replacing the line which includes the non-zero Dirichlet boundary condition with the boundary conditions itself, i.e. for a system of 3 equations:

$$
	\begin{bmatrix}
	  K_{11} & K_{12} & K_{13}\\
	  K_{21} & K_{22} & K_{23}\\
	  K_{31} & K_{32} & K_{33}\\
	\end{bmatrix}
	\begin{bmatrix}
	  u_1\\u_2\\u_3
	\end{bmatrix}
	=
	\begin{bmatrix}
	  f_1\\f_2\\f_3
	\end{bmatrix}
$$

The boundary condition $u_2 = \Delta_2$ can be inserted:

$$
	\begin{bmatrix}
	  K_{11} & K_{12} & K_{13}\\
	  0 & 1 & 0\\
	  K_{31} & K_{32} & K_{33}\\
	\end{bmatrix}
	\begin{bmatrix}
	  u_1\\u_2\\u_3
	\end{bmatrix}
	=
	\begin{bmatrix}
	  f_1\\\Delta_2\\f_3
	\end{bmatrix}
$$

Which can be further simplified to:

$$
	\begin{bmatrix}
	  K_{11} & 0 & K_{13}\\
	  0 & 1 & 0\\
	  K_{31} & 0 & K_{33}\\
	\end{bmatrix}
	\begin{bmatrix}
	  u_1\\u_2\\u_3
	\end{bmatrix}
	=
	\begin{bmatrix}
	  f_1 - K_{12} \ \Delta_2 \\\Delta_2\\f_3  - K_{32} \ \Delta_2
	\end{bmatrix}
$$

## Example

Let us use what we have just learned on a simple example:

```{figure} twobarqdiri.svg
:name: twobarqdiri
:align: center
```

This example has the same two-element bar model as in [](../lecture1/directly.md) and [](./element_loads.md), so the stiffness matrix is unchanged, including the support reactions:

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
\begin{array}{c|cc}
\class{cA}{\cfrac{EA_1}{\ell_1}} + \class{cB}{\cfrac{EA_2}{\ell_2}} &  \class{cA}{-\cfrac{EA_1}{\ell_1}}  & \class{cB}{-\cfrac{EA_2}{\ell_2}}\\
\hline
\class{cA}{-\cfrac{EA_1}{\ell_1}} & \class{cA}{\cfrac{EA_1}{\ell_1}} & 0\\
\class{cB}{-\cfrac{EA_2}{\ell_2}} & 0 & \class{cB}{\cfrac{EA_2}{\ell_2}}\\
\end{array}
\right]
\begin{bmatrix}
u_2\\[12pt] \hline  0 \\[12pt] \bar u
\end{bmatrix}
=
\begin{bmatrix}
 \class{cA}{\cfrac{q\ell_1}{2}} \class{cB}{+ \cfrac{q\ell_2}{2}} \\ \hline  
 H_1 \class{cA}{ + \cfrac{q\ell_1}{2}}\\
 H_3 \class{cB}{ + \cfrac{q\ell_2}{2}}
\end{bmatrix}
$$

with:

- $\mathbf{K}_\mrm{ff} =  \class{cA}{\cfrac{EA_1}{\ell_1}} + \class{cB}{\cfrac{EA_2}{\ell_2}} $
- $\mathbf{K}_\mrm{fc} =  \begin{bmatrix} \class{cA}{-\cfrac{EA_1}{\ell_1}}  & \class{cB}{-\cfrac{EA_2}{\ell_2}} \end{bmatrix}$
- $\mathbf{K}_\mrm{cf} =  \begin{bmatrix} \class{cA}{-\cfrac{EA_1}{\ell_1}} \\ \class{cB}{-\cfrac{EA_2}{\ell_2}} \end{bmatrix}$
- $\mathbf{K}_\mrm{cc} =  \begin{bmatrix}\class{cA}{\cfrac{EA_1}{\ell_1}} & 0\\  0 & \class{cB}{\cfrac{EA_2}{\ell_2}}\\ \end{bmatrix}$

Now, the unknown $\mathbf{u}_\mrm{f}$ (including only $u_2$) can be solved for using the equations provided in [](static-condensation).

### Size-preserving approach

Now let's apply the alternative approach. First, let's set the $u_1 = 0$:

$$
	  \begin{bmatrix}
	1 & 0 & 0\\
	0 & \class{cA}{\cfrac{EA_1}{\ell_1}} + \class{cB}{\cfrac{EA_2}{\ell_2}} & \class{cB}{-\cfrac{EA_2}{\ell_2}}\\
	0 & \class{cB}{-\cfrac{EA_2}{\ell_2}} & \class{cB}{\cfrac{EA_2}{\ell_2}}\\
	  \end{bmatrix}
	  \begin{bmatrix}
	u_1\\[12pt]u_2\\[12pt]u_3
	  \end{bmatrix}
	  =
	  \begin{bmatrix}
	0 \\ \class{cA}{\cfrac{q\ell_1}{2}} \class{cB}{+ \cfrac{q\ell_2}{2}}\\H_3 \class{cB}{ + \cfrac{q\ell_2}{2}}
	  \end{bmatrix}
	  $$

No terms are added to the force vector as this enforced displacement is $0$.

Let's continue with the displacement $u_3 = \bar u$:

$$
	  \begin{bmatrix}
	1 & 0 & 0\\
	0 & \class{cA}{\cfrac{EA_1}{\ell_1}} + \class{cB}{\cfrac{EA_2}{\ell_2}} & 0\\
	0 & 0 & 1 \\
	  \end{bmatrix}
	  \begin{bmatrix}
	u_1\\[6pt]u_2\\[6pt]u_3
	  \end{bmatrix}
	  =
	  \begin{bmatrix}
	0 \\ \class{cA}{\cfrac{q\ell_1}{2}} \class{cB}{+ \cfrac{q\ell_2}{2}} - \class{cB}{\cfrac{EA_2}{\ell_2}} \bar u\\ \bar u
	  \end{bmatrix}
	  $$

Now, a term is added to the force vector, as $\mathbf{K}_{23}$ wasn't equal to 0 (and now it is).

This matrix equation can now be solved for $\mathbf{u}$.