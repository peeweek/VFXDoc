# Performance Overview

Performance is often seen as the worst enemy of quality in video gaming. It involves the best set up you can achieve for maximum acceptable frame-rate with the fewest visual quality sacrifices to achieve that. 

While it is often seen as a constraint, performance is also a big asset as it will involve one of the biggest quality triggers of all : framerate.

As effect are animated, it is crucial to target and maintain a good balance between visual quality and frame-rate because:

* Simulation at low framerate becomes jagged and artifacts start to appear

* Frame rate determines the feel of the game, and thus your effects:

  * 60 fps feel smooth, frantic and heady
  * 30 fps feel ok for most games but can feel a bit laggy for fast paced-action
  * 24 fps is ok for cinematic, but will involve laggy and heavy gameplay
  * Anything below 20 fps will feel annoying if kept for too long.
  * Below 8 fps, an eye loses track of any motion

* Moreover, a player's eye is pretty keen on frame rate variations. So any frame drop can be seen as annoying, and if repeated too often can end up in bad feeling of a game.

## Performance, Target and Budget

Depending on the type of game we are making and the platforms we are developing the game for, the game team and the CTO will decide on two major performance targets:

* A reference platform (for instance PlayStation 4)
* A reference rendering resolution (for instance 1080p)
* A reference framerate (often 30fps or 60fps, rendering for VR platforms can go up to 90fps)

### Minimal Configuration

If the project targets include hardware-varying platforms such as PC or Android, it is often advised to perform an assessment of the minimal configuration depending on your target audience and define this minimal configuration as your target.

For other multiple platforms, the game team shall probably choose in any case the weaker of all platforms as reference in order to save heavy production optimizations, and specific content cuts. 

This is often an underestimated constraint which costs a lot of effort and frustration on the team. Because of targeting a more powerful platform, it becomes harder to remove critical content for weaker platforms, than adding eye candy and pushing all levers to 11 on powerful platforms.

### Target Frame Rate

Target frame rate is also critical and do not need to be underestimated. Some games display graphics of a certain quality, with a lot of content in wide areas, that will require performance-consuming rendering features. These games will often target 30fps to be able to run all the content with good quality.

Of course, your team will want the best for the game : 60fps is a must. But it has also to be balanced with the visual quality. Running a game at 60fps is not something that has to be taken lightly.

### Target Resolution

## Performance 101

### Milliseconds vs FPS

### Game Threads and Performance

### VSync or not VSync

## How to ruin Game Performance in 10 steps with VFX

### Blended Overdraw

### Non-Blended Overdraw

### Off Screen Drawing and Simulation

### Texture Cache Stalls

### Particle Lighting

### Vertex Pressure

### Invisible Costs

### Texture Multi-Taps and Dependency

### Screen-Space Derivatives

### Post-Processing

## How to save Game Performance in 10 steps with VFX

### Half-Resolution Transparency

### Particle Cropping

### Pixel Ratio

### Shader Optimizations

### Lighting Optimizations



### 



