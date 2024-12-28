# Transformations

Up until now we didn't care about the orientation of elements. Actually, all elements had exactly the same orientation. But how do deal with elements in a different orientation?

> You'll look into how to transform elements from a local to global orientation

The stiffness matrix is defined in a local coordinate system following an element's orientation. This is useful because it allows us to reuse that same stiffness matrix over and over again without the need to rederive it. However, during assembly, when combining multiple elements, it would be useful to have them all in the same coordinate system, the global one. After solving for displacements in the global coordinate system, it might be needed to transform back to the local coordinate system to get expression for continuous displacements and section forces.

```{figure} transformations.svg
:name: transformations
:align: center
```

We'll be using a $x-z$-coordinate system as the differential equations are derived using those axes. This is different than in most finite-element-implementations and in most cases not in line with international standards.

# Transformation for an arbitrary vector

For an arbitrary vector $v$ with two components, the transformation matrix can be derived by comparing the vector's components in the local ($v_{\bar x}, v_{\bar z}$) and global ($v_x,v_z$) coordinate system:

```{figure} transform_1.svg
:name: transform_1
:align: center
```

The relations between the components can be found using geometry:

```{figure} transform_2.svg
:name: transform_1
:align: center
```

Leading to:

$$
{\cB\color{cB}v_{\bar{x}}} = {\cA\color{cA}v_x} \cos\alpha - {\cA\color{cA}v_z} \sin \alpha \\
{\cB\color{cB}v_{\bar{z}}} = {\cA\color{cA}v_x} \sin\alpha + {\cA\color{cA}v_z} \cos \alpha
$$

This can be rewwritten in matrix form as:

$$ {\cB\color{cB}
      \begin{bmatrix}
	v_{\bar{x}} \\ v_{\bar{z}}
      \end{bmatrix}
      }
      {\color{black}
      =
      \underbrace
      {
	\begin{bmatrix}
	  \cos\alpha & -\sin\alpha \\ \sin\alpha & \cos\alpha\\
	\end{bmatrix}
      }
      _\mathbf{R}
      }
      {\cA\color{cA}
      \begin{bmatrix}
	v_x \\ v_z
      \end{bmatrix}
      }$$

And the inverse relation:

$$
{ \cA\color{cA}
      \begin{bmatrix}
	v_x \\ v_z
      \end{bmatrix}
      }
      {\color{black}
      =
      \underbrace
      {
	\begin{bmatrix}
	  \cos\alpha & \sin\alpha \\ -\sin\alpha & \cos\alpha\\
	\end{bmatrix}
      }
      _{\mathbf{R}^\mrm{T}}
      }
      {\cB\color{cB}
      \begin{bmatrix}
	v_{\overbar{x}} \\ v_{\overbar{z}}
      \end{bmatrix}
      }
$$
