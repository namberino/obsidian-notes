# Overview

Often, just describing the system of interest is not enough, we want to control the system actively to change its behaviors. There's a lot of different types of controls: passive control, active control, open loop control, closed loop feedback control.

# Linear Systems

$$
\dot{x} = Ax
$$

$x$ is a vector with $n$ elements, $A$ is an $n\times n$ matrix that tells us how the variables interact with each other and how the derivative of $x$ is dependent on the state vector. The basic solution of this is below:

$$
x(t) = e^{At} x(0)
$$

The $e^{At}$ term is calculated with the Taylor series of the exponential with the $At$ plugged in.

$$
e^{At} = I + At + \frac{A^2t^2}{2!} + \frac{A^3t^3}{3!} + ...
$$

It's not practical to compute this. Instead, we use the eigenvalues and eigenvectors of $A$ to get a coordinate transformation that will be applied onto $x$. This transformation will make it easier to compute $e^{At}$ and understand the dynamics.

$$
A \xi = \lambda \xi
$$

With $\xi$ being the eigenvector and $\lambda$ being the eigenvalue. So we can characterize this eigenvalues and eigenvectors in the form of matrices instead of vectors:

$$
\begin{aligned}
&A T = T D \rightarrow T^{-1} A T = D
\\
T &= \begin{bmatrix} | & | & & | \\ \xi_1 & \xi_2 & ... & \xi_n \\ | & | & & | \end{bmatrix}
\\
D &= \begin{bmatrix} \lambda_1 & 0 & ... & 0 \\ 0 & \lambda_2 & ... & 0 \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & ... & \lambda_n \end{bmatrix}
\end{aligned}
$$

It's not always true that we can write the systems in this eigenvectors and eigenvalues form, sometimes the eigenvectors are generalized, sometimes we have a Jordan form, etc. However, this eigenvectors and eigenvalues form is the most common case.

$$
x = Tz
$$

We can transform the system's state vector into this new eigencoordinate specified by the eigenvectors in $T$. $z$ is the system's state vector in this new eigencoordinate and because $T$ is invertible, we can apply $T$ on $z$ to get $x$.

Now, we can write our system's dynamics in terms of this new eigencoordinate:

$$
\begin{aligned}
&\dot{x} = T \dot{z} = A x
\\
&T \dot{z} = A Tz
\\
&\dot{x} = T^{-1} A T z = D z
\end{aligned}
$$

The dynamics of the system becomes diagonal in the eigencoordinate. The components of $z$ are completely decoupled from each other and $\dot{z_n} = \lambda_n z_n$.

$$
\frac{d}{dt} \begin{bmatrix} z_1 \\ z_2 \\ \vdots \\ z_n \end{bmatrix} = \begin{bmatrix} \lambda_1 & 0 & ... & 0 \\ 0 & \lambda_2 & ... & 0 \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & ... & \lambda_n \end{bmatrix} \begin{bmatrix} z_1 \\ z_2 \\ \vdots \\ z_n \end{bmatrix}
$$

The solution to this systems of equation is this:

$$
\begin{aligned}
z(t) &= e^{Dt} z(0)
\\
&= \begin{bmatrix} e^{\lambda_1 t} & 0 & ... & 0 \\ 0 & e^{\lambda_2 t} & ... & 0 \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & ... & e^{\lambda_n t}\end{bmatrix} z(0)
\end{aligned}
$$

We can write the dynamics of $x$ in terms of this eigenvectors relationship:

$$
A = TDT^{-1}
$$

We'll use this to simplify the Taylor series expression (with $TT^{-1}$ being the identity matrix).

$$
\begin{aligned}
&e^{At} = I + At + \frac{A^2t^2}{2!} + \frac{A^3t^3}{3!} + ...
\\
&e^{TDT^{-1} t} = TT^{-1} + TDT^{-1}t + \frac{(TDT^{-1})^2t^2}{2!} + \frac{(TDT^{-1})^3t^3}{3!} + ...
\\
&e^{TDT^{-1} t} = TT^{-1} + TDT^{-1}t + \frac{TD^2T^{-1}t^2}{2!} + \frac{TD^3T^{-1}t^3}{3!} + ...
\end{aligned}
$$

So $A^n = TD^n T^{-1}$. Notice how each term has $T$ on the left and $T^{-1}$ on the right. We can now factor them out:

$$
\begin{aligned}
e^{TDT^{-1} t} &= T \biggr[ I + Dt + \frac{D^2t^2}{2!} + \frac{D^3t^3}{3!} + ... \biggl] T^{-1}
\\
&= T e^{Dt} T^{-1}
\end{aligned}
$$

Now we can conclude an efficient way of computing the $e^{At}$ term.

$$
e^{At} = T e^{Dt} T^{-1}
$$

$e^{Dt}$ is very easy to compute as it's just a diagonal matrix. Now we can plug in this $e^{At}$ term into the solution of the differential equation:

$$
x(t) = T e^{Dt} T^{-1} x(0)
$$

Remember that $x = T z$, so mapping $x(0)$ through $T^{-1}$ would give us $z(0)$, which is the initial condition of the system in the eigencoordinate.

$$
x(t) = T e^{Dt} z(0)
$$

The $e^{Dt}$ term will advance the initial conditions of the system through time until we hit time $t$. So $e^{Dt}z(0)$ will give us $z(t)$.

$$
x(t) = T z(t)
$$

And T will just transform $z(t)$ from the eigencoordinate back into the original coordinate system $x(t)$.

# Stability

Stability has a lot to do with the eigenvalues (which can be complex) of the matrix $A$, which is heavily utilized in $e^{Dt}$. If any of the terms in the diagonal matrix $e^{Dt}$ blows up to infinity, there's a high probability that $x(t)$ will also blow up to infinity.

Each of the $\lambda$ value in the diagonal matrix has a real and imaginary part. We can imagine these eigenvalues in the complex plane.

$$
\begin{aligned}
&\lambda = a + ib
\\
&e^{Dt} = e^{at} [\cos(bt) + i \sin(bt)]
\end{aligned}
$$

If $a \gt 0$, the system is growing exponentially to $\infty$. If $a \lt 0$, the system is decaying exponentially to 0. So the system is stable if and **only if** all the real parts $a$ in all the eigenvalues $\lambda_n$ are negative, otherwise, the system is unstable. So if the system is unstable, a couple eigenvalues are positive, we can add a control term $+ Bu$ to the system $\dot{x} = Ax$ to try to drive the unstable eigenvalues to the negative side.

A real-life system is not continuous, it's measured in discrete time. With $x_k = x(k \Delta t)$, we have:

$$
x_{k+1} = \tilde{A} x_k
$$

This is a discrete update of the system, updating the state vector at time $k$ to time $k + 1$ with a time step of $\Delta t$. $\tilde{A}$ is how the $A$ matrix will update the dynamics based on the time step.

$$
\tilde{A} = e^{A\Delta t}
$$

In discrete time, there's also stability but it's a bit different. If we have $x_0$, we can compute trajectory of the system and all possible future states of the system by continuously multiply that initial state with $\tilde{A}$.

$$
x_N = \tilde{A}^N x_0
$$

So the same thing will happen in this discrete time case as in the continuous time case, $\tilde{A}$ can be expanded to $T e^{D \Delta t} T^{-1}$, and the $T$ matrix is just a coordinate transformation. $x_0$ will be multiplied with the diagonal matrix itself. What we find is that the eigenvalues will grow or decay. In the complex plain, the radius of the eigenvalue will expand or shrink.

So if the value of $\lambda$ is less than 1, the system is stable, else, if it's larger than 1, it's unstable.

![](./Assets/stability-discrete-time-complex-plane.png)

Recap: The stability of the system is completely dependent on the eigenvalues of $A$. For continuous time systems, if the eigenvalues are **all** negative, the system is stable. For discrete time systems, if **all** the eigenvalues have a magnitude less than 1, the system is stable.