# The Game Pre-Production

### Define your workflow

Defining your workflow will be the most important task of the pre-production, alongside working with art direction if your game has really demanding needs for visual effects, which is sometimes happening. Your workflow will stick to your skin all production long so you better be prepared for the worst if you do not want to end up overwhelmed by repetitive tasks, or worse, having to over-engineer things in a really simple project.

The workflow you will be defining can vary from really simple straightforward tasks to fully fledged automation in order to process large amounts of vfx, but more importantly it will define the succession of software, scripts and tasks required for the creation, import, authoring, integration, validation, and debug of all your effects.

#### Software & Pipeline

The pre-production is a really cool time where you can evolve the software pipeline and make It more efficient in order to spend less time in unnecessary tasks, and spare this time improving quality and performance of your effects.

If you have written a postmortem of your previous production, you can open it at the section labeled « when my software let me down ». Postmortems are very valuable for you because you can forget from one production to another some of the major bottlenecks you encountered.

Most of the time, out-of-engine bottlenecks come from the lack of features from one software to another, leading to setting up a two-or-three-software-pipeline : a succession of actions in several successive software that will be in charge of a part of your data processing.

#### Optimization Trump Cards

When making VFX you often have to face the choice of letting go one of your dearly loved effect because it has become the weak link in the frame, and costs too much to stay alive. If your engine does not have a fully featured translucent pipeline or a lot of safeguards for your particles, shaders & renderers, pre-production is often a good time to ask engine programmers to implement these safeguard features.

These safeguards could be (non exhaustive) :

- Half Resolution Translucency
- LDR Rendering
- Particle LODs
- Near-Camera Particle Fading & Culling

You can also perform performance analysis really early in production by learning the bottlenecks and how to bypass through-the-roof millisecond values by reducing the rest of the scene’s cost. To do so, It is a good idea to start acquiring a good knowledge of your engine debug views, and know how to use profiling tools.

(More of these are covered in the Performance Chapter at the end of this book.)

#### Production Loads

Let’s take the following example : Your game requires to setup a process on all meshes of all levels to set up a geometric deconstruction effect. To do so, you will have to be clever and come up with a plan, not only for you but also for the other teams. The plan could involve the following :

- Asking Engine programmers to provide in-engine tools to automate data generation or import. As well as providing architecture advice in order for your data to be balanced, performance-wise.
- Asking other Tech Artists for pipeline tricks or scripts if you are stuck by doing everything by yourself. (It’s not a shame to ask, especially if this is identified as your main bottleneck).
- Teaming up with environment artists to figure out their process, how to fit in It, how to react when they make changes, and what could be the technical limitations of their process (for instance, if the process require fully closed meshes)
- Teaming up with game design & level design to acquire metrics data, test out your effects in prototype condition in order to evaluate if your solution is viable when used in gameplay condition.
- Communicating with production to help them provide the workload charge you will have to spend in order to handle the production (whether automated or not) of all these effects.

In the end of the pre-production, you should have a clearer idea of what you will be doing, how you will do It and most importantly how many time it will take and the performance impact on the game.

### Prepare your Data and Hard Drives.

### Start early tests on platform

One thing to have ready at all times is a proper test environment. A lot of projects start with a not-so-good setup for the team and once the production starts, it becomes difficult for the team to allocate time to setup and test. This section will cover the generic usage of debug tools such as Xbox One (Durango) SDK or PS4 DevKit, but there are also instructions for every platform. If you are unsure, please contact your CTO (if any), or your device provider (Microsoft, Sony, Apple, etc.)

The first step is to set-up a PC/Mac test environment. This is the easiest thing to do, and if your engine is already up an running (Unreal, Unity,…) there are already the required software to save your project, export a test build and run It on your machine. Unreal relies on a software named UnrealFrontend to generate builds, and it's really easy to understand, the software proposes builds for all the correctly-configured targets and it's a one-click-and-go-make-coffee button. Unity has slightly the same but directly into the editor in the File>Build Settings, where you can access platform export settings, content to package and options for export.

Development builds vs. Release builds : What's important to know is what will be included in your build. There is some lingo associated with builds and depending on what you will build, you will be able -or not- to debug or access information for your effects. Development builds are extended builds of your game containing debug tools in order to evaluate problems or benchmark performance. Often, these builds have worse performance than the final game. Because of the embedding of performance markers, profilers, and debug information, the executable is performing additional tasks that are unnecessary for the game, but will be helpful for you. Also, these tools weigh a bit of more RAM and can lead for your game to crash if you are really close to the memory limit in release builds.

Here are some examples of different kind of builds :

| Type                  | Description                              | Performance Impact                       |
| --------------------- | ---------------------------------------- | ---------------------------------------- |
| FinalRelease          | Game executable stripped of all debug code and tools. Bare minimum with code optimizations. | Best, will be the build of your published game. |
| Test Release          | Game executable stripped of all debug code and tools, often, with some code to help devs check framerate and memory. | Best, but embeds unnecessary code to verify performance. |
| Development / Release | Game executable with optimizations but embedding debug tools and connections to profiler (markers in code) | Close to finalrelease, but performance drawbacks will impact more on this version. |
| Debug                 | Game executable compiled in debug mode (with debug symbols), performance markers, debug tools, and connectors to profiler. | Worse, far from anything you could expect from your game performance. Used for programmers to debug their code. |



### Your relationship with CTO