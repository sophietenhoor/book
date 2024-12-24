# Example - 3D frame with torsion

This page shows the final part of the 3D frame example which we didn't finish in class.

## Solving stiffness Matrix
In class, we found the complete stiffness matrix:

$$\left[ \begin{matrix}
   2000 & 1000 & 0 & 0 & 0  \\
   1000 & 4400 & 1000 & -400 & 0  \\
   0 & 1000 & 2400 & 0 & -400  \\
   0 & -400 & 0 & 400 & 0  \\
   0 & 0 & -400 & 0 & 400  \\
\end{matrix} \right] \left[ \begin{matrix}
   {{\varphi }_{1}}  \\
   {{\varphi }_{2}}  \\
   {{\varphi }_{3}}  \\
   {{\varphi }_{4}}  \\
   {{\varphi }_{5}}  \\
\end{matrix} \right]=\left[ \begin{matrix}
   {{M}_{1}}  \\
   2  \\
   4  \\
   {{M}_{t,4}}  \\
   2+{{M}_{t,5}}  \\
\end{matrix} \right]$$

## Solving boundary conditions
As we've no non-zero boundary conditions, we can apply the row-striking technique of lecture 1, which leads to:

$$\left[ \begin{matrix}
   4400 & 1000 \\
   1000 & 2400
\end{matrix} \right] \left[ \begin{matrix}
   {{\varphi }_{2}}  \\
   {{\varphi }_{3}} 
\end{matrix} \right]=\left[ \begin{matrix}
   2  \\
   4 
\end{matrix} \right]$$

Solving this system of equations gives:

$$\varphi_2 \approx 1 \cdot 10^{-4} \\
\varphi_3 \approx 1.6 \cdot 10^{-3}$$

The support reactions can be found by inserting these into our original system of equations:

$$\left[ \begin{matrix}
   2000 & 1000 & 0 & 0 & 0  \\
   
   0 & -400 & 0 & 400 & 0  \\
   0 & 0 & -400 & 0 & 400  \\
\end{matrix} \right] \left[ \begin{matrix}
   0  \\
   1 \cdot 10^{-4}   \\
   1.6 \cdot 10^{-3}  \\
   0  \\
   0  
\end{matrix} \right]=\left[ \begin{matrix}
   {{M}_{1}}  \\
   {{M}_{t,4}}  \\
   2+{{M}_{t,5}}  \\
\end{matrix} \right]$$

Note that to calculate $M_{t,4}$ the element load should be taken into account!
This gives:

$$M_1 \approx 0.1 \text{ kNm} \\
M_{t,4} \approx -0.033 \text{ kNm} \\
M_{t,5} \approx -2.7 \text{ kNm}$$

## Postprocessing moments element $\left(1\right)$

The continuum displacement field of element $\left(1\right)$ can be described by the shape function:

$$w\left(x\right) = \left( -\cfrac{x^3}{\ell^2} + \cfrac{x^2}{\ell} \right) \varphi_2 $$

This gives:

$$\varphi \left(x\right) = -\cfrac{\text{d} w\left(x\right)}{\text{d}x}=\left( \cfrac{3x^2}{\ell^2} - \cfrac{2x}{\ell} \right) \varphi_2 \\
\kappa \left(x\right) = \cfrac{\text{d} \varphi\left(x\right)}{\text{d}x}=\left( \cfrac{6x}{\ell^2} - \cfrac{2}{\ell} \right) \varphi_2 \\
M \left(x\right) = EI \kappa=EI\left( \cfrac{6x}{\ell^2} - \cfrac{2}{\ell} \right) \varphi_2 \\$$

This is a linear distribution, with values at $x=0$ and $x=\ell$:

$$M \left(0\right) \approx -0.1 \text{ kNm} \\
M \left(2\right) \approx 0.2 { kNm} $$

The value at $x=0$ has indeed the same absolute value as the support reactions. The sign is different because $M_1$ is defined in the global coordinate system and $M \left(0\right)$ is defined from our agreements on positive internal moments (leading to positive stresses at the positive $z$-side.)