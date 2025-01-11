# Matrix multiplication

Applying 2 transformation is called *composing*. The resulting transformation after applying 2 transformation (1 after the other) is called a *composition*. This transformation can be describe using a matrix by following $\hat{i}$ and $\hat{j}$.

The composition matrix is the product of the 2 other transformation matrices. For example, applying a rotation followed with a shear means the composition matrix is the product of the rotation matrix and the shear matrix. 

Example (Read from right to left for the left hand side, first matrix from the right is the first transformation. Then multiplication is taking the left matrix and multiply it with $\hat{i}$ and $\hat{j}$ on the matrix on the right):

$$
\begin{bmatrix} a & b \\ c & d \end{bmatrix}
\begin{bmatrix} e & f \\ g & h \end{bmatrix} = 
\begin{bmatrix} ae + bg & af + bh \\ ce + dg & cf + dh \end{bmatrix}
$$

> Note: ORDER MATTERS FOR THIS

For 3D space, there's 3 basis vectors for this: $\hat{i}$ for the X-axis, $\hat{j}$ for the Y-axis, $\hat{k}$ for the Z-axis.

Example:

$$
\begin{bmatrix} 1 & 1 & 1 \\ 0 & 1 & 0 \\ -1 & 0 & 1 \end{bmatrix}
$$

Figuring out vector transformation based on the basis vector transformation is similar to the 2D version:

$$
\vec{v} = \begin{bmatrix} x \\ y \\ z \end{bmatrix} = x \hat{i} + y \hat{j} + z \hat{k}
$$

The 3D composition multiplication one is similar to the 2D version:

$$
\begin{bmatrix} a & b & c \\ d & e & f \\ g & h & i \end{bmatrix} 
\begin{bmatrix} j & k & l \\ m & n & o \\ p & q & r \end{bmatrix} =
\begin{bmatrix} aj + bm + cp & ak + bn + cq & al + bo + cr \\ dj + em + fp & dk + en + fq & dl + eo + fr \\ gj + hm + ip & gk + hn + iq & gl + ho + ir \end{bmatrix} 
$$

# Inverse matrix

With an inverse matrix $A^{-1}$, if you apply a transformation with the matrix $A$, and apply the inverse matrix, you land back where you started. So multiplying the 2 matrix together gives us a matrix that does nothing. This is called an *identity transformation*. 

$$
A^{-1} A = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}
$$

The inverse transformation only exist if the determinant of $A$ is non-zero. When the determinant $A$ is 0, there is no inverse.

This can be used for calculating systems of equations.

$$
A \vec{x} = \vec{v}
$$

For systems of equations, even when the determinant of $A$ is 0, there could still be a solution, but you'd have to be really lucky that the $\vec{v}$ lands somewhere on that line.

![](./Assets/soe-det-solution-zero.png)

# Non-square matrix

Example: 3x2 matrix. A 3x2 matrix has the geometric interpretation of mapping 2 dimensions to 3 dimensions since the 2 columns indicate that the input space has 2 basis vectors and the 3 rows indicate that the landing spots for each basis vectors after the transformation (3 coordinates = 3 dimensions). 

$$
\begin{bmatrix} 3 & 1 \\ 4 & 1 \\ 5 & 9 \end{bmatrix}
$$

You can go from 2D to 3D and vice versa (though 3D to 2D is pretty weird if you think about it). It's also possible to go from 2D to 1D since 1D is just a number line. A transformation like this would just take in 2D vectors and spit out numbers.

A 1D transformation can be encoded with a 1x2 vector, with the first number being the number $\hat{i}$ landed on and the second number being the number $\hat{j}$ landed on (This has close ties to the dot product).

$$
\begin{bmatrix} 1 & 2 \end{bmatrix}
$$