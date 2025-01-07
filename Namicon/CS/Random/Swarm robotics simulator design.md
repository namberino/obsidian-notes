![](./Assets/components-design)

- Robot model: Using URDF would be nice and standard but might be a little too much for a 6-month project. For now I think implementing the models in code would be easier and quicker than building a URDF interpreter. Base robot model will be just a simple cube with wheels (maybe a differential driver).
- World generation: Perlin noise to generate the terrains for the world. The pseudo-randomness of Perlin noise would allow us to test the robots' exploration and mapping capabilities. We start with a flat plane to test out other aspects of the simulator first, then work on world generation later.
- Graphics and physics engine: This is a no brainer. Just need some simple collision physics though, no need for something complex.
- User + Control algorithms: The user should be able to implement control algorithms and run those control algorithms in the simulator. Implementing algorithms and simulating those algorithms should be easy for the user to do.
