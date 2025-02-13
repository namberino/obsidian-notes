# Data-driven Dynamical Systems

Dynamical system:

$$
\dot{x} = f(x, t, u, \beta)
$$

With $x$ being the system's state, $t$ is time, $u$ is some control input, $\beta$ are the parameters (some systems has a lot of parameters), and $f$ is the dynamics (vector field because it's a vector, telling us how fast $x$ is changing and in which direction is it changing to).

In some field, there's no physics equation that capable of modeling the system that that field is researching. However, there are more and more data in those fields. We can utilize those data to build models. First principle physics model wouldn't be applicable to building models of these kinds of systems, we'll have to use data.

Big problems in modern dynamical systems: Linear systems are fully understood. Nonlinear systems ($f$) makes it hard to understand and control the system. Sometimes $f$ is unknown and we need to discover it from data. High dimensional systems uses very high dimensional state vector (large $x$). Chaotic and transient systems, noisy and stochastic measurements, multi-scale dynamics, uncertainty in systems are also challenging.

Uses of models: Predicting future states, design and optimization, control, etc.

Techniques: Regression (Linear, sparse, etc), Deep learning, Genetic programming, etc.