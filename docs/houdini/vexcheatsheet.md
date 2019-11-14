# Houdini VEX Cheat Sheet

## Syntax

Variable (generic):  `@foo;`

Assign Variable: `@foo = 1;`

Declarations : 

```c
float @mass = 1;
vector @up = {0, 1, 0};
```

#### Conditionals

```c
if ( rand(@ptnum) > ch('threshold') ) {
   removepoint(0,@ptnum);
}
```

#### Functions

```c
int test(int a, b; string c) {
      // ...
}
```

You can introduce a function definition with the optional function keyword to avoid type ambiguity.

```c
function int test(int a, b; string c) {
      // ...
}
```

#### Loops

| Type                    | Declaration                              |
| ----------------------- | ---------------------------------------- |
| do loop                 | ` do {statement} [while (condition)]`    |
| foreach loop            | ` foreach (value; array) {statement}`    |
| foreach loop with index | `foreach (index, value; array) {statement}` |
| while                   | ` while (condition) {statement}`         |
| foreach point           | `forpoints ( point ) {statement}`        |
| return                  | `return value;`                          |
| break                   | `break;`                                 |
| continue                | `continue;`                              |



#### Preprocessing

The compiler has a pre-processor which strips comments, reads include files, and expands macros.

The pre-processor supports many of the usual C Pre-Processor directives:

* `#define name token-string` Replace subsequent uses of name with token-string.


* `#define name(arg,...,arg) token-string` Replace subsequent instances of name with token-string. Each argument to name is replaced in token-string during expansion.


* `#undef name` "Undefine" the macro so subsequent uses of name are not expanded.
* `#include "filename"` Inserts the contents of the file at this point in the source code. When you use quotes, the directory containing the current file is searched for filename before the standard locations (including the path).


* `#ifdef name` The following lines until the next `#endif` or `else` directive will be compiled if and only if name is a defined macro.


* `#ifndef name` The lines following will be compiled if and only if name is *not* a defined macro.


* `#if constant-expr` The following lines until the next `#endif` or `#else` directive will be compiled if and only if constant-exprevaluates to non-zero.

  The expression can use the following operators:

  * Comparisons (`==`, `!=`, `<=`, `>=`, `<`, `>`)
  * Logical AND (`&&`), OR (`||`), and NOT (`!`).
  * Bitwise AND (`&`), OR (`|`), exclusive OR (`^`), and NOT (`~`).
  * Arithmetic (`+`, `-`, `*`, `/`, `%`).
  * Parentheses.

  The expression can also use the following functions:

  Expressions are evaluated from left to right (**unlike the ANSI C standard of right to left**). As with the ANSI pre-procssor, all numbers must be integers.

* `#else` The following lines until the next `#endif` directive will be compiled if and only if the previous `#if`directive evaluated to zero.

* `#endif` Marks the end of a section of conditional code. Every test directive must have a matching `#endif`.

* `#pragma ...` Specifies extended language features. See the [list of pragmas](http://www.sidefx.com/docs/houdini/vex/pragmas).



## Attributes (Builtin)

#### Geometry

| Attrib     | Def                       |
| ---------- | ------------------------- |
| `@ptnum`   | Point index               |
| `@numpt`   | Total count of points     |
| `@primnum` | Primitive Index           |
| `@numprim` | Total count of primitives |

#### Temporal

| Attrib     | Def                                |
| ---------- | ---------------------------------- |
| `@Time`    | Current time (float), in seconds   |
| `@Frame`   | Current Frame (int)                |
| `@TimeInc` | Time delta per Frame (1/framerate) |



## Types

| Type      | Declaration | Definition                               | Example                                  |
| --------- | ----------- | ---------------------------------------- | ---------------------------------------- |
| `int`     | i@var       | Integer values                           | `21, -3, 0x31, 0b1001, 0212, 1_000_000`  |
| `float`   | f@var       | Floating point scalar values             | `21.3, -3.2, 1.0, 0.000_000_1`           |
| `vector2` | u@var       | Two floating point values. You might use this to represent texture coordinates (though usually Houdini uses vectors) or complex numbers | `{0,0}, {0.3,0.5}`                       |
| `vector`  | v@var       | Three floating point values. You can use this to represent positions, directions, normals or colors (RGB or HSV) | `{0,0,0}, {0.3,0.5,-0.5}`                |
| `vector4` | p@var       | Four floating point values. You can use this to represent positions in homogeneous coordinates, or color with alpha (RGBA) | `{0,0,0,1}, {0.3,0.5,-0.5,0.2}`          |
| `array`   |             | A list of values. See [arrays](http://www.sidefx.com/docs/houdini/vex/arrays) for more information. | `{ 1, 2, 3, 4, 5, 6, 7, 8 }`             |
| `struct`  |             | A fixed set of named values. See [structs](http://www.sidefx.com/docs/houdini/vex/lang#structs) for more information. |                                          |
| `matrix2` | 2@var       | Four floating point values representing a 2D rotation matrix | `{ {1,0}, {0,1} }`                       |
| `matrix3` | 3@var       | Nine floating point values representing a 3D rotation matrix or a 2D transformation matrix | `{ {1,0,0}, {0,1,0}, {0,0,1} }`          |
| `matrix`  | 4@var       | Sixteen floating point values representing a 3D transformation matrix | `{ {1,0,0,0}, {0,1,0,0}, {0,0,1,0}, {0,0,0,1} }` |
| `string`  | s@var       | A string of characters. See [strings](http://www.sidefx.com/docs/houdini/vex/strings) for more information. | `"hello world"`                          |
| `bsdf`    |             | A *bidirectional scattering distribution function*. See [writing PBR shaders](http://www.sidefx.com/docs/houdini/vex/pbr) for information on BSDFs. |                                          |

#### Casts

implicit `@var`, Explicit `v@vec`

```c
// BAD 
@Cd = @mycolour; // This will treat it as a float and only read the first value 
				 //(ie, just the red channel of @mycolour) 

// GOOD
@Cd = v@mycolour; // Explicitly tells the wrangle that its a vector.
```



#### Arrays

Declaration : `i[]@array = neighbours(0, @ptnum);`

Vectors are arrays : `@P = v@foo;`

```c
v@foo.x = 1;
v@foo.r = 1;
v@foo[0] = 1;
```



## Parameters

Float `ch('name')`

Vector`chv('name')`

Ramp `chramp('name', sampleValue)`

Upon adding a parameter, click the button on the far right to expose it to the Wrangle.



## Functions

#### Utility

Range fitting `fit(value, min-input, max-input, min-output, max-output)`

Length `length(@vector)`

Random `rand(@ptnum)`

SinCos `sin(@radians)` `cos(@radians)`

Convert Degrees to Radians `radians(@angleInDegrees)`

Distance `distance(@source, @target)` 



#### Grouping

Adding a point to a group named `group1` is achievable by setting a float value;

Syntax: `f@group_<groupName>` so for our group it would be `f@group_group1`

Example:

```
if(angle < ch('threshold'))
{
  f@group_group1 = 1.0;
}
```



#### Point Cloud search

Point cloud find, Returns close points based on a position, search radius and max count, ordered by proximity.

 `int[] pcfind(int inputnum, string pchannel, vector pos, float searchRadius, int maxpoints);`

Iteration on pcfind:

```
int points[] = pcfind(0, "P", chv('center'), ch('radius'), 10 );
foreach(int point; points)
{
	setpointattrib(0, "Cd", point, {1,0,0});
}
```



#### Altering Geometry (Must be run in CVEX context )

You can create geometry with VEX snippets in certain nodes, such as [Attrib Wrangle](http://www.sidefx.com/docs/houdini/nodes/sop/attribwrangle). To be able to create geometry, the node must run in the `cvex` context, which means the [Point Wrangle](http://www.sidefx.com/docs/houdini/nodes/sop/pointwrangle) **cannot** create geometry since it runs in the `sop` context. 

```c
int p1 = addpoint(geoself(), {0, 1, 0});
int p2 = addpoint(geoself(), {1, 1, 0});
int p3 = addpoint(geoself(), {1, 1, 1});

int prim = addprim(geoself(), "poly");
addvertex(geoself(), prim, p1);
addvertex(geoself(), prim, p2);
addvertex(geoself(), prim, p3);
```





## Troubleshooting error messages

| Your code            | Problem                                  |
| :------------------- | :--------------------------------------- |
| `force += 2`         | `Syntax error, unexpected '}', expecting ';'`Each statement must end with a semicolon (`;`) |
| `@v += force`        | `SRead-only expression given for read/write parameter`Particle nodes cannot modify particle attributes. |
| `x = { 0, @y, 0}`    | `Syntax error, unexpected identifier, expecting '}'.`You cannot have varying arguments to the `{}` vector constructor. Use `set()` instead: `x = set(0, @y, 0);`. |
| `x = set(0, $F, 0);` | `Doesn't animate.`While `$F` will be evaluated, VOP networks are not time dependent so it won’t animate. Use `@Frame` instead. |