Transformation basically take in some inputs and spit out some outputs. But what sets it apart from function is you can think of it as transforming or moving the vector. Making it point in other directions with different magnitude.

Arbitrary transformation can get pretty complex. Luckily, linear algebra limits itself to linear transformations.

Linear transformation properties:
- All lines must remain lines (No curves)
- The origin remains fixed in place

Linear transformation keeps the grid lines parallel and evenly spaced.

This transformation is described with the transformation of $\hat{i}$ and $\hat{j}$:

$$
\begin{aligned}
\vec{v} = \begin{bmatrix} -1 \\ 2 \end{bmatrix} = -1 \hat{i} + 2 \hat{j}
\\
\text{Transformed } \vec{v} = -1 (\text{Transformed } \hat{i}) + 2 (\text{Transformed } \hat{j})
\end{aligned}
$$

So we have this:

$$
\begin{bmatrix} a \\ b \end{bmatrix} \rightarrow a \begin{bmatrix} x_i \\ y_i \end{bmatrix} + b \begin{bmatrix} x_j \\ y_j \end{bmatrix} = \begin{bmatrix} ax_i + bx_j  \\ ay_i + by_j \end{bmatrix}
$$

It's common to group the 2 transformed basis vectors into a 2x2 matrix. With this matrix, we can determine the transformation of any vector in a 2D space.

$$
\begin{bmatrix} x_i & x_j \\ y_i & y_j \end{bmatrix}
$$

So given any vector's coordinate, we can determine the new vector's coordinate after the transformation.

$$
\begin{bmatrix} x_i & x_j \\ y_i & y_j \end{bmatrix} \; \begin{bmatrix} a \\ b \end{bmatrix} \rightarrow a \begin{bmatrix} x_i \\ y_i \end{bmatrix} + b \begin{bmatrix} x_j \\ y_j \end{bmatrix} = \begin{bmatrix} ax_i + bx_j  \\ ay_i + by_j \end{bmatrix}
$$

This is *matrix - vector multiplication*.

With linearly dependent transformation, the transformation squishes all of 2D space into 1 line where the 2 vectors sit.

$$
\begin{bmatrix} 2 & -2 \\ 1 & -1 \end{bmatrix}
$$

# Transformation ranks

Ranks (The number of dimensions in an output transformation):
- Rank 1: The output of the transformation is a line (1D)
- Rank 2: The output of the transformation is a 2D plane
- Rank 3: The output of the transformation is a 3D space

The set of all possible outputs for a matrix is called *column space* of the matrix. The column space is the span of all the columns in the transformation matrix. So the rank is the number of dimensions in the column space. When the rank is equal to the number of columns in the matrix, it's "full rank".

> Note: The zero vector will always be in the column space since all linear transformation must keep the origin fixed.

The set of vectors that land on the origin after the transformation is called the *null space* or the *kernel*.