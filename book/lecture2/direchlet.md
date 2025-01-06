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