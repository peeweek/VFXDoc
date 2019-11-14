# Introduction to Houdini



### Architecture

Houdini is architectured about many interconnected networks of processing. All these networks are typed and respond to specific needs with specific tools. We encounter many kind of graphs and every one of them has specific semantic and order of processing.

Within a HIP (houdini project) file, all the data is organized in a hierarchy quite similar to a UNIX filesystem. This means that every node in your project can be accessed by using a path such as this one `/obj/Sphere01/scatter01` 

This example tells us that the scatter01 node is located inside the Sphere01 node which lies into the /obj domain. One thing important to note is that graphs can be nested inside nodes.



| Type                           | Where | Description                              |
| ------------------------------ | ----- | ---------------------------------------- |
| **Object**                     | /obj  | Objects are located in the /obj folder of your file hierarchy |
| **SOP** (Surface Operator)     | /obj  | SOPs are contained inside some object operators. |
| **DOP** (Dynamics Operator)    | /obj  | DOPs are Simulation contexts that retain state between frames. They are used to simulate fire,smoke, particles, fluids, rigid bodies,... |
| **SHOP** (Shader Operator)     | /shop | SHOPs are Shader operators. They are set on one or many objects and used for rendering. |
| **ROP** (Render Operator)      | /out  | ROPs are used for outputting data in a render environment, these nodes can render images, geometry or other data in a dependency graph. |
| **COP** (Compositing Operator) | /img  | COPs are Compositing operators that work on sequences of images taken directly from ROPs or from disk. |
| **CHOP** (Channel Operator)    | /ch   | Channel Operators are used to create and modify curve data (audio, animations) |