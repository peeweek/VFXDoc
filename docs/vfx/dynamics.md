# Dynamics and Motion

Most of visual effects rely on physical concepts to animate and give a credible and gracious visual effect. Most of the effects are driven by using terms and parameters pulled straight from the field of physics. Here, we will go through basic concepts of physics to understand the basics of the phenomenon and how they can be of use when authoring effects.

## General Physical Concepts

Particle systems are commonly used for physical effects, but tend to approximate what you could achieve precisely with physical rigid bodies (either computed real time or baked in mesh animations). Anyway, being approximate does not imply that the way to describe behaviors will be different. Here’s a quick lexicon of the terms used in effects dynamics.

### Body Properties

<video loop="true" autoplay="true" ><source  src="../img/ball-bounce.mp4" type='video/mp4' /></video>

**Body :** an evolving body, that can be a simple particle or a more complex body (rigid body, collision primitive)

**Velocity **: Velocity is the vector expression of the motion of a body, and is often described as distance travelled by a body in a lapse of time. Its vector definition makes it define a direction and a magnitude.

**Speed **: Slightly different from velocity, and often confused with. Speed is considered as the magnitude of the velocity. Magnitude is the length of a vector so if for example our 2D particles have a velocity of (X: 3, Y: 4) magnitude of the velocity will be the length of this vector which would be 5 .

| Figure showing the pythagorian magnitude. | ![img](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEYAAAAWCAIAAADywn0kAAACDElEQVR4nO2XPWvCQBjHL+BHEPoFEkGxXyCiQ7coBeng2i0pdEgWN8EI3VziUGhuK+0UaCkUzSYUMVunSAvqFyj0O6R5UfN28eW8oC39L8KZe3K/+z/Pc5eMZVngbylz6AU4mvdKjFQcWCpHItoxIM37mgH4VpCHoiiMQF7GHQFShOjzuaOZsixvmvUqP74j/zk8EsKj00b7Ip8843t0dzsE5/LTA/KxZCQ3wY3gCE8q20OafUSJtpX5Nqrky9noMBrJw2GVmSXSq5FLwGC8d5P0FwhYhWRkJJLete3hB+Mlj6dijkY9vJ88ohrJyNvWEi2OVdx3BFLYNj60USkQoZGYAgsArAr1NbUTL7WYlrVnb4cl2r+6QFWlri76QdMgQiPR4r2iMRKsUjC2r/4z3jp3ENdUWCjd9JrcImAqREmJ5yy45vpgSAwlJXHtKFps8ZJv1Hw6AWyDNNG6WvJ8cLIF2lwlQIIqaJR7IBVbxHvOxvbAqdas4Nil9efi/ky+Uc1p7IgloxiSLghADXcFutZgY51gh/YQ1sIoAYB0iGJITn7nomNOhgA+fCxhtIfVTMcoCEEikal1TKzQrqJI9vUESqVCoG50gXEPXoJ3Ic8og6+jQuYrZyfm8As/ehSJU2fKhHG6nD9G/m631uJs+eoa2BdTXKp4e8DPKHKyqdpl3MmH/7ggrn+k36Af0G70ZKIZBloAAAAASUVORK5CYII=) |
| ---------------------------------------- | ---------------------------------------- |
|                                          |                                          |

**Mass** : A property of weight that involves a resistance to acceleration. Expressed in grams 

**Spin / Angular Velocity :**

Angular velocity of a body, or its spin, is the self-rotation velocity of the object. Spin is often described as the magnitude of the angular velocity (just as Speed is the magnitude of the momentum). In complex systems, for non-uniform bodies, Angular velocity makes the drag coefficient vary.

### Medium Properties

**Density**

**Viscosity**



### Forces

**Force :** an external phenomenon or an interaction of the body with its environment that will have consequence on its velocity.

**Net Force :** the overall force acting on an object. It is bascially the sum of all the interactions the body has with its medium.

**Weight / Gravity: **The universal force on planets that makes everything fall, even apples. Basically a constant force applied to all objects depending of the density of the medium. This force is applied on every object depending on its Mass.

**Buoyancy : **The Archimede’s principle force applied to physic bodies: The buoyancy is an inverse force applied to light bodies to drag them upwards because of differences of density. Inside water, a bubble of air will rise towards the surface because of its density, way less dense that water. In the air, cigarette smoke tend to rise up when being emitted from the tip of the cigarette because of temperature (hot air is less dense than cold air and tends to buoy), then flows less upwards as it cools down.

**Drag **: In layman’s terms : air resistance.

But more precisely : medium resistance. This is a force resulting of the interaction between a moving object and the medium where It evolves. This force depends on the density of the medium and a coefficient based on the shape and the orientation of the evolving object.

The drag force can be reduced to a force which is directed towards the body, at every point and that is more intense as the surface is facing the direction of the velocity.

> Example : A leaf and a small wooden stick will fall at different speeds because of the drag coefficient of the falling leaf that is superior to the stick’s. However, the leaf’s drag coefficient will be different whether the leaf is falling [on the side], or [flatly].

Here’s a figure of various shapes and their drag coefficients.

*(Figure showing shapes and their drag coefficients)*

**Lift :** a force perpendicular to the direction of the relative motion. This is another force applied with moving bodies, related to the shape of the object and the pressure.

> For example : wings of a plane will divert air flow coming frontwards, and cause lift force to make the plane resist vertically enough to maintain its altitude.

**Orbit :** Orbit force is a physical attraction of celestial bodies (planets) an is often encountered in particle systems. However, the intent behind the feature is not the same as a behavior of celestial bodies. It is often considered as a force applied to the particle to make it revolve around a position, at a given speed, and around a specified rotation axis.

It is often used to make particles revolve as a tornado or used in conjunction to velocity to make particles jitter from their trajectory, and act as they are being dragged into a turbulent field. (figure neeeded)

Some particle engines also call this feature Vortex, Particle offset with angular velocity.

### Collision Physics

**Friction :** Friction is often mislead with drag as the two terms seems to have slightly the same meaning. In terms of particles, Friction is often used to describe the tangential dampening when a body collides with another. Drag could be described as air friction if we consider that our flying body is in permanent collision with the air.

**Dampening :** the inverse of “Bounciness”, a factor of energy absorption when a body collides with another. In terms of physics the energy loss by dampening is based on the two bodies dampening coefficient.



## Physical Concepts in VFX

While bodies in physics are described with full properties and coherent medium, particle systems in game engines have been historically using fast approximations and incoherent properties to achieve results. Nevertheless the concepts that apply are often inherited from real-life physics.

### Positions and Velocity Integration

Positions for particles define how they evolve spatially along time. As time in video games is spliced into frames, positions are updated at different moments, with given lapse of time between each computation : the **Delta Time**. This value is utterly important when it comes to update pretty much everything in a simulation as it will serve as a multiplier to everything in order to apply a part of the simulation along time (for instance a part of the velocity).

Velocity Integration is the operation where we displace every particle by its speed, given a lapse of time. The general equation for this is :

<center>`position += velocity * deltaTime;`</center>

as the velocity is expressed in units per second, and deltaTime is a fraction of a second that represents the time between two frames, we need to displace particle given this fraction.

#### Precision in integration

As the velocity is integrated once per lapse, the greater deltaTime becomes, the less precise the simulation will be. A good example for this is a particle which trajectory is erratic because of a high frequency velocity turbulence.

If the velocity changes 30 times per second, but the delta Time is only 1/10th of a second, instead of catching 30 velocity changes per second, it will catch only 1 change out of 3. Which will ignore the two others.

In the end, an effect played at a framerate of 10 images per second and an effect played at a framerate of 30 frames per second will diverge from their given trajectories, because their velocity integration is different.

### Forces, Velocity And Speed

Forces are modifiers for the velocity. They apply to the particle additively every frame and modify it so the velocity can now be applied to the positions (integration

)

### Rotation and Spin



### Color



### Forces



Offsets



Collision





## Physically-Based Particles

