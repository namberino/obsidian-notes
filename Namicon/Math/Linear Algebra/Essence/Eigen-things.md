# Eigenvectors and eigenvalues

Consider some linear transformation in 2D, take a particular vector on the space before the transformation and consider the span of that vector (the line passing through that vector). During the transformation, most vectors would get knocked off their span. However, some vectors do remain on their own span. So the transformation only squished or stretched that vector like a scalar.

Example:

$$
\begin{bmatrix} 3 & 1 \\ 0 & 2 \end{bmatrix}
$$

For this transformation matrix, the vector that doesn't get knocked off its span is the $\hat{i}$ vector. Because of the way linear transformation works, any other vectors on the span of the $\hat{i}$ vector also remains on that span, they only get stretched out by 3 in this example.

Another vector like this in this transformation is $\begin{bmatrix} -1 \\ 1 \end{bmatrix}$. This one gets stretched by a factor of 2. So any other vector on the span of that vector is also going to remain on that span after the transformation, and get stretched by a factor of 2.

For this transformation, those are the only 2 vectors that has that special property, any other vectors will get knocked off of its own span after the transformation.

These special vectors are called the *eigenvectors* of the transformation. Each eigenvector is associated with an *eigenvalue*, which is the scaling factor of the vector, how much the vector got squished or stretched during the transformation.

Negative eigenvalues means the vector gets flipped.

# Eigenvector in 3D

In a 3D rotation, if we can find an eigenvector for that rotation, then we've found the axis of rotation. When it comes to thinking about 3D rotation, it's easier to think of the axis of rotation and the angle by which it rotates around that axis rather than think of the 3x3 matrix associated with the transformation.

In the case of rotation, the eigenvalue will be 1, since rotation never squishes or stretches anything. 

# Eigenvector for interpreting transformation

Often, the transformation matrix is interpreted as the landing spots for the basis vectors. But this gives too much reliance on the coordinate system in use.

A better way to get to the heart of what the linear transformation does is finding the eigenvectors and eigenvalues.

# Computation

Here's how eigenvectors can be describe:

$$
A\vec{v} = \lambda\vec{v}
$$

Where:
- $A$: The transformation matrix
- $\vec{v}$: The eigenvector
- $\lambda$: The eigenvalue

This basically tells us that the matrix - vector multiplication product is the same as scaling that vector up with a scalar value.

To make it easier to work with (since the left hand side is matrix - vector multiplication and the right hand side is scalar - vector multiplication), we can turn the scalar $\lambda$ into a matrix representation. The common way to write this is to name the identity matrix $I$ and write $\lambda I$

$$
\begin{aligned}
&\lambda = \begin{bmatrix} \lambda & 0 & 0 \\ 0 & \lambda & 0 \\ 0 & 0 & \lambda \end{bmatrix} = \lambda \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix} = \lambda I
\\
&A\vec{v} = (\lambda I) \vec{v}
\end{aligned}
$$

Now with both sides looking like matrix - vector multiplication, we can subtract the right hand side and factor out $\vec{v}$ to this:

$$
(A - \lambda I) \vec{v} = \vec{0}
$$

We now have a new matrix $A - \lambda I$, which looks something like this:

$$
\begin{bmatrix} 3 - \lambda & 1 & 4 \\ 1 & 5 - \lambda & 9 \\ 2 & 6 & 5 - \lambda \end{bmatrix}
$$

Now we need to find a vector $\vec{v}$, such that when multiplied with the new matrix, gives the $\vec{0}$ vector. We want a non-zero solution for this.

The only way this can happen is if the determinant for the new matrix is 0 (squishes space into a lower dimension). Because a 0 determinant matrix would squish all space into a lower dimension, it would also squish the vector $\vec{v}$ into a $\vec{0}$ vector.

Because we need the $\vec{v}$ to be squished into a $\vec{0}$ vector, we need to pick a lambda value such that it makes the determinant 0. The determinant is the scaling factor of any arbitrary region in space, so when its 0, all regions gets squished into a lower dimension, including our vector.

We can tweak the $\lambda$ value such that it squishes the transformation into a lower dimension, meaning the determinant will be 0. So there's a non-zero vector $\vec{v}$ that satisfies these conditions.

Example: Calculating the eigenvectors and eigenvalues

$$
\begin{bmatrix} 3 & 1 \\ 0 & 2 \end{bmatrix}
$$

To find the eigenvalue $\lambda$, we first subtract it from the diagonals of this transformation matrix:

$$
\begin{bmatrix} 3 - \lambda & 1 \\ 0 & 2 - \lambda \end{bmatrix}
$$

Then we compute the determinant:

$$
\det \biggr( \begin{bmatrix} 3 - \lambda & 1 \\ 0 & 2 - \lambda \end{bmatrix} \biggl) = (3 - \lambda) (2 - \lambda) - 1 \cdot 0 = (3 - \lambda) (2 - \lambda)
$$

Because there can only be eigenvalues and eigenvectors if the determinant is 0. So for this instance, the only possible eigenvalues are $\lambda = 3$ and $\lambda = 2$.

To figure out the eigenvectors that actually have these eigenvalues, plug in the eigenvalues into the matrix and solve for this system of equations:

$$
\begin{bmatrix} 3 - \lambda & 1 \\ 0 & 2 - \lambda \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \end{bmatrix}
$$

This corresponds to the fact that the unaltered matrix would scale the vector by 2 or 3, so subtracting the eigenvalue would make the matrix scale the vector by 0. Hence the $\begin{bmatrix} 0 \\ 0 \end{bmatrix}$.

Not all transformation has an eigenvector. Case in point: rotations. For example, a rotation by $90\degree$ gives the eigenvalues $\lambda = i$ and $\lambda = -i$, which are imaginary numbers. So there are no eigenvectors.

Another example is the shear:

$$
\begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix}
$$

The only eigenvector for this is the $\hat{i}$ vector and its span with an eigenvalue of 1:

$$
\det \biggr( \begin{bmatrix} 1 - \lambda & 1 \\ 0 & 1 - \lambda \end{bmatrix} \biggl) = (1 - \lambda) (1 - \lambda) \rightarrow \lambda = 1 \text{ for det = 0}
$$

A single eigenvalue can have more than just 1 line of eigenvectors.

$$
\begin{bmatrix} 2 & 0 \\ 0 & 2 \end{bmatrix}
$$

This transformation scales everything by 2. The only eigenvalue is 2 but since everything is scaled by 2, every vector is an eigenvector with that eigenvalue.

# Eigenbasis

Example: Both of our basis vectors are eigenvectors.

$$
\begin{bmatrix} -1 & 0 \\ 0 & 2 \end{bmatrix}
$$

Anytime a matrix has 0s everywhere except on the diagonal line, it's called the diagonal matrix. This type of matrix also has a property that all of the basis vectors in this matrix is an eigenvector with the diagonal entry of the matrix being their eigenvalues.

Diagonal matrices is much easier to compute with (Example: applying a matrix many times, like 100 times).

But what are the odds of the basis vectors becoming eigenvectors? If your matrix has a lot of eigenvectors, enough so that you can choose a set that spans the full space, then you can change your coordinate system so that these eigenvectors are your basis vectors.

Example: Let's take this matrix our new basis vectors, which are eigenvectors (written in our coordinate system)

$$
\begin{bmatrix} 1 & -1 \\ 0 & 1 \end{bmatrix}
$$

And we want to transform it with this matrix (written in our coordinate system):

$$
\begin{bmatrix} 3 & 1 \\ 0 & 2 \end{bmatrix}
$$

So new we can do the transformation translation:

$$
\begin{bmatrix} 1 & -1 \\ 0 & 1 \end{bmatrix}^{-1} \begin{bmatrix} 3 & 1 \\ 0 & 2 \end{bmatrix}\begin{bmatrix} 1 & -1 \\ 0 & 1 \end{bmatrix} = \begin{bmatrix} 3 & 0 \\ 0 & 2 \end{bmatrix}
$$

This gives us the transformation matrix in the new coordinate system. This transformation matrix is guaranteed to be a diagonal matrix. Since the eigenvectors that are the basis vectors of this new coordinate system will only get scaled up or down during the transformation.

The set of eigenvectors that are also basis vectors is called *eigenbasis*.

So if we need to compute this:

$$
\begin{bmatrix} 3 & 1 \\ 0 & 2 \end{bmatrix}^{100}
$$

Then it's easier to change to an eigenbasis, compute the 100th power, then change back to the original basis. Not all matrices can become diagonal matrices since they don't have enough eigenvectors to span the full space.

# Eigenvalues computing trick

> Note: This is a trick for 2x2 matrices

3 facts:
1) $\frac{1}{2} \text{trace} \biggr(\begin{bmatrix} a & b \\ c & d \end{bmatrix} \biggl) = \frac{a + d}{2} = \frac{\lambda_1 + \lambda_2}{2} \rightarrow m$
2) $\det \biggr(\begin{bmatrix} a & b \\ c & d \end{bmatrix} \biggl) = ad - bc = \lambda_1\lambda_2 \rightarrow p$
3) $p = m^2 - d^2 = (m+d)(m-d) \rightarrow d^2 = m^2 - p \rightarrow \lambda_1, \lambda_2 = m \pm \sqrt{m^2 - p}$