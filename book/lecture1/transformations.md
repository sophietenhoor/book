# Transformations

Up until now we didn't care about the orientation of elements. Actually, all elements had exactly the same orientation. But how do deal with elements in a different orientation?

> You'll look into how to transform elements from a local to global orientation

The stiffness matrix is defined in a local coordinate system following an element's orientation. This is useful because it allows us to reuse that same stiffness matrix  and  again without the need to rederive it. However, during assembly, when combining multiple elements, it would be useful to have them all in the same coordinate system, the global one. After solving for displacements in the global coordinate system, it might be needed to transform back to the local coordinate system to get expression for continuous displacements and section forces.

```{figure} transformations.svg
:name: transformations
:align: center
```

We'll be using a $x-z$-coordinate system as the differential equations are derived using those axes. This is different than in most finite-element-implementations and in most cases not in bar with international standards.

# Transformation for an arbitrary vector

For an arbitrary vector $v$ with two components, the transformation matrix can be derived by comparing the vector's components in the local ($v_{\bar x}, v_{\bar z}$) and global ($v_x,v_z$) coordinate system:

```{figure} transform_1.svg
:name: transform_1
:align: center
```

The relations between the components can be found using geometry:

```{figure} transform_2.svg
:name: transform_2
:align: center
```

Leading to:

$$
\class{cB}{v_{\bar{x}}} = \class{cA}{v_x} \cos\alpha - \class{cA}{v_z} \sin \alpha \\
\class{cB}{v_{\bar{z}}} = \class{cA}{v_x} \sin\alpha + \class{cA}{v_z} \cos \alpha
$$

This can be rewwritten in matrix form as:

$$ \class{cB}{
      \begin{bmatrix}
	v_{\bar{x}} \\ v_{\bar{z}}
      \end{bmatrix}
      }
      {
      =
      \underbrace
      {
	\begin{bmatrix}
	  \cos\alpha & -\sin\alpha \\ \sin\alpha & \cos\alpha\\
	\end{bmatrix}
      }
      _\mathbf{R}
      }
      \class{cA}{
      \begin{bmatrix}
	v_x \\ v_z
      \end{bmatrix}
      }$$

And the inverse relation:

$$
\class{cA}{
      \begin{bmatrix}
	v_x \\ v_z
      \end{bmatrix}
      }
      {
      =
      \underbrace
      {
	\begin{bmatrix}
	  \cos\alpha & \sin\alpha \\ -\sin\alpha & \cos\alpha\\
	\end{bmatrix}
      }
      _{\mathbf{R}^\mrm{T}}
      }
      \class{cB}{
      \begin{bmatrix}
	v_{\bar{x}} \\ v_{\bar{z}}
      \end{bmatrix}
      }
$$

## Transformation for a complete element

To transform a complete element, the displacements of both endpoints have to be transformed, while the rotations are independent of the element orientation:

$$
      \class{cB}{
	\begin{bmatrix}
	  \bar{u}_1\\\bar{w}_1\\\bar{\varphi}_1\\
	  \bar{u}_2\\\bar{w}_2\\\bar{\varphi}_2\\
	\end{bmatrix}
      }
      =
      \underbrace
      {
	\begin{bmatrix}
	  \cos\alpha & -\sin\alpha & 0 & 0 & 0 & 0\\
	  \sin\alpha &  \cos\alpha & 0 & 0 & 0 & 0\\
	  0 & 0 & 1 & 0 & 0 & 0\\
	  0 & 0 & 0 & \cos\alpha & -\sin\alpha & 0\\
	  0 & 0 & 0 & \sin\alpha &  \cos\alpha & 0\\
	  0 & 0 & 0 & 0 & 0 & 1\\
	\end{bmatrix}
      }_\mathbf{T}
      \class{cA}{
	\begin{bmatrix}
	  {u}_1\\{w}_1\\{\varphi}_1\\
	  {u}_2\\{w}_2\\{\varphi}_2\\
	\end{bmatrix}
      }
$$

Resulting in:

- ${\class{cB}\bar{\mathbf{u}}} = \mathbf{T}{\class{cA}\mathbf{u}}$
- ${\class{cB}\bar{\mathbf{f}}} = \mathbf{T}{\class{cA}\mathbf{f}}$
- ${\class{cA}{\mathbf{u}}} = \mathbf{T}^\mrm{T}{\class{cB}\bar{\mathbf{u}}}$
- ${\class{cA}{\mathbf{f}}} = \mathbf{T}^\mrm{T}{\class{cB}\bar{\mathbf{f}}}$

## Transformation for stiffness matrix

Using the known transformation for the first-order tensors $\mathbf{u}$ and $\mathbf{f}$ the transformation matrix for the second-order tensor $\mathbf{K}$ can be derived:

$$
\class{cB}{\bar{\mathbf{K}}} {\class{cB}\bar{\mathbf{u}}} = {\class{cB}\bar{\mathbf{f}}} \\
\class{cB}{\bar{\mathbf{K}}} \mathbf{T}{\class{cA}\mathbf{u}} = \mathbf{T}{\class{cA}\mathbf{f}} \\
\mathbf{T}^\mrm{T}\class{cB}{\bar{\mathbf{K}}} \mathbf{T}{\class{cA}\mathbf{u}} = \mathbf{T}^\mrm{T}\mathbf{T}{\class{cA}\mathbf{f}} 
\class{cA}{\bar{\mathbf{K}}}{\class{cA}\mathbf{u}} = {\class{cA}\mathbf{f}} 
$$

So $\class{cB}{\bar{\mathbf{K}}} = \mathbf{T}^\mrm{T}\class{cB}{\bar{\mathbf{K}}} \mathbf{T}$