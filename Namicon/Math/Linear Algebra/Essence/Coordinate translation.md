# Basis vectors

The typical way to think about coordinate in linear algebra is thinking of them as scalars. The scalar values in the vector scales the unit vectors up to create a new vector.

This is tied up with the choice of the basis vectors as the vectors that the scalars are meant to scale with.

Coordinate system is a way to translate sets of numbers into vectors. 1 thing that's consistent with all coordinate systems is the origin, the origin is what you get when you scale any vector by 0.

# Translating between coordinate systems

When we want to translate to different coordinate systems, it helps to think of this act as transforming our basis vectors to match the other coordinate system. For example, we have a different coordinate system that's basically a transformed version of our coordinate system defined by this matrix. Below is the other coordinate system's basis matrix written in our coordinate system:

$$
\begin{bmatrix} 2 & -1 \\ 1 & 1 \end{bmatrix}
$$

For this coordinate system, that's their basis vectors, their $\begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$. So how do we translate a vector drawn in their coordinate system, say $\begin{bmatrix} -1 \\ 2 \end{bmatrix}$, to our coordinate. We can transform our coordinate to match their coordinate then draw out that vector based on their coordinate while still keeping track of our coordinate. This is essentially matrix - vector multiplication.

$$
\begin{bmatrix} 2 & -1 \\ 1 & 1 \end{bmatrix} \begin{bmatrix} -1 \\ 2 \end{bmatrix} = \begin{bmatrix} -4 \\ 1 \end{bmatrix}
$$

So we get the coordinate of their $\begin{bmatrix} -1 \\ 2 \end{bmatrix}$ vector in our coordinate system.

Doing the opposite, we can translate our vector in our coordinate system into a different coordinate system. To do this, we can start with the basis matrix that translate the other coordinate system to ours, then we take its inverse.

$$
\begin{bmatrix} 2 & -1 \\ 1 & 1 \end{bmatrix}^{-1} = \begin{bmatrix} 1/3 & 1/3 \\ -1/3 & 2/3 \end{bmatrix}
$$

So if we have a vector in our coordinate system, say $\begin{bmatrix} 3 \\ 2 \end{bmatrix}$, we can multiply it with the inverse basis matrix to translate it into the other coordinate system:

$$
\begin{bmatrix} 1/3 & 1/3 \\ -1/3 & 2/3 \end{bmatrix}\begin{bmatrix} 3 \\ 2 \end{bmatrix} = \begin{bmatrix} 5/3 \\ 1/3 \end{bmatrix}
$$

# Translating transformation

How do we know how a transformation works in a different coordinate system? For example, given a new coordinate system, how do we know what a $90\degree$ transformation look like?

We can do this by first getting the other coordinate system's transformation matrix (change of basis matrix) in our coordinate system:

$$
\begin{bmatrix} 2 & -1 \\ 1 & 1 \end{bmatrix}
$$

This allows us to transform our basis vectors to match the other coordinate system's basis vectors. Then we multiply it with the transformation (which performs the transformation). We'll use the $90\degree$ transformation for this:

$$
\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}
\begin{bmatrix} 2 & -1 \\ 1 & 1 \end{bmatrix}
$$

This allows us to emulate how the transformation would look like in the other coordinate system in our coordinate system. At last, now we want to translate back and see where the basis vectors landed in the other coordinate system after the transformation. To do this, we just multiply with the inverse matrix of the change of basis matrix:

$$
\begin{bmatrix} 2 & -1 \\ 1 & 1 \end{bmatrix}^{-1}
\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}
\begin{bmatrix} 2 & -1 \\ 1 & 1 \end{bmatrix}
$$

And this gives us the transformation matrix in the other coordinate system. So now if we want to calculate how a vector would be transformed given a transformation, say rotated by $90\degree$, we can do this:

$$
\begin{bmatrix} 2 & -1 \\ 1 & 1 \end{bmatrix}^{-1}
\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}
\begin{bmatrix} 2 & -1 \\ 1 & 1 \end{bmatrix}
\vec{v}
$$

Example: The below shows us how the $\begin{bmatrix} 1 \\ 2 \end{bmatrix}$ in the other coordinate system gets transformed when a $90\degree$ transformation is applied (Remember that the end coordinate is in the other coordinate system, not our coordinate system).

$$
\begin{bmatrix} 2 & -1 \\ 1 & 1 \end{bmatrix}^{-1}
\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}
\begin{bmatrix} 2 & -1 \\ 1 & 1 \end{bmatrix}
\begin{bmatrix} 1 \\ 2 \end{bmatrix} = \begin{bmatrix} -1 \\ 1 \end{bmatrix}
$$

So we have this formula for calculating transformations in other coordinate systems:

$$
A^{-1}MA
$$