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