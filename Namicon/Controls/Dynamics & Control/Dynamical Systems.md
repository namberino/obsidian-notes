# Data-driven Dynamical Systems

In some field, there's no physics equation that capable of modeling the system that that field is researching. However, there are more and more data in those fields. We can utilize those data to build models. First principle physics model wouldn't be applicable to building models of these kinds of systems, we'll have to use data.

Big problems in modern dynamical systems: Linear systems are fully understood. Nonlinear systems ($f$) makes it hard to understand and control the system. Sometimes $f$ is unknown and we need to discover it from data. High dimensional systems uses very high dimensional state vector (large $x$). Chaotic and transient systems, noisy and stochastic measurements, multi-scale dynamics, uncertainty in systems are also challenging.

Uses of models: Predicting future states, design and optimization, control, etc.

Techniques: Regression (Linear, sparse, etc), Deep learning, Genetic programming, etc.

> Note: All systems have some constraints. We can enforce these constraints in these systems to improve the models.

# Anatomy of Dynamical Systems

Dynamical systems describe the evolving world around us. It's linked to how a system change in time.

$$
\dot{x} = \frac{d}{dt}x = f(x, t, u; \beta) + d
$$

Where:
- $x$ being the system's state (minimal description vector with the minimal number of values needed to describe the system)
- $t$ is time (lots of dynamical systems change in time)
- $u$ is control input (actuation, some variables we have control over that can change the system)
- $\beta$ are the parameters of the system that we don't have control over (some systems has a lot of parameters, we can understand the reliance of the system on these parameters)
- $d$ is the external disturbances in the system (most dynamical systems have this)
- $f$ is the dynamics. A set of functions that describe the dynamics of each of the states (It's a vector field because it's a vector, telling us how fast $x$ is changing and in which direction is it changing to - which direction the derivative of $x$ is pointing in space)

Sometimes we don't have access to $x$ because it's exceedingly large, so we have some small subset of measurements $y$ (with $n$ being the noise in the system).

$$
y = g(x, t) + n
$$

# The Lorenz System

One of the earliest chaotic models. It's a 3D model that gets a lot of the features of chaotic atmospheric convection.

$$
\begin{aligned}

\dot{x} &= \sigma (y - x)

\\

\dot{y} &= x (\rho - z) - y

\\

\dot{z} &= xy - \beta z

\\

\underline{\beta} &= \begin{bmatrix} \sigma & \rho & \beta \end{bmatrix}^T, \underline{x} = \begin{bmatrix} x & y & z \end{bmatrix}^T

\end{aligned}
$$

With $\underline{\beta}$ being the parameters vector and $\underline{x}$ being the state vector.

$$
\frac{d}{dt} \underline{x} = f(\underline{x}, t, \underline{\beta})
$$

# Discrete-Time Dynamical Systems

Continuous time system:

$$
\dot{x} = f(x(t))
$$

Discrete time system (more general than continuous time systems):

$$
\begin{aligned}
&x_{k + 1} = F (x_k)
\\
&x_k = x(k \Delta t)
\end{aligned}
$$

Example: We're sampling an evolving population. We sample the system every day, so $\Delta t$ might be 1-day sampling, $k$ would be the current day (day 1, day 2, day 3, etc).

Flow map ($F_{\Delta t}$):

$$
x_{k+1} = x_k + \int_{k\Delta t}^{(k+1)\Delta t} f(x(\tau)) d\tau
$$

Integrate the trajectory through the dynamical system for 1 $\Delta t$ to get the next $x$. Flowing the state from 1 point to another. The integral term evaluates the vector field $f$ as the state $x$ evolves.

We can always go from a continuous time system to a discrete time system, but not always back.

Simple flow map approximation with forward Euler:

$$
\begin{aligned}
&\frac{x_{k + 1} - x_k}{\Delta t} \approx f(x_k)
\\
&x_{k+1} = x_k + \Delta t f(x_k)
\end{aligned}
$$

In general, for most nonlinear dynamical systems, the flow map is hard to compute, so we'll have to approximate it numerically with schemes like forward Euler, Runge-Kutta, etc.
