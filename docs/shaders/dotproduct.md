# The Dot Product Operation

Dot product (a.k.a Scalar Product) is a pretty common math algebraic operation used in trigonometry that helps solving many cases. It works by comparing two vectors of any length and returns a value based on their length and the angle they are forming.

## Principles

### Behaviors

The dot product compares two vectors based on their length and the angle they form with the following behaviors:

* With two normalized vectors:
  * Returns a **co-linearity coefficient**: which means how much the two vectors are parallel and in the same direction:
    * 1.0 if they are **equal**
    * 0.0 if they are **perpendicular**
    * -1.0 if they are **opposed** (exact opposite directions)
  * **Projects** the end of the first vector on the second vector and determines the length of its projection
* With two non-normalized vectors:
  * **Projects** the end of the first vector on the second vector and determines the length of its projection
* With only one vector compared to itself:
  * Determines the **squared length** of the vector.

Its properties are the following :

* **Symmetric**: Meaning that A dot B equals B dot A

### Formula

The dot product of two vectors `v1` and `v2` can be computed by the following formula:

`dot(v1,v2) = len(v1)*len(v2)*cos(angle(v1,v2))`

It can also be resolved by the following formula:

`dot(v1,v2) = (v1.x * v2.x) + (v1.y * v2.y) + (v1.z * v2.z)`

Thus resolving the cosine of the angle without using an actual cosine, if the lengths of v1 and v2 are known.

## Use Cases

Dot product is used in many cases

### Squared length of a Vector

### Co-linearity of two normalized Vectors

### Projection of a point on an Axis (Closest point to the axis)

## Practical Cases

### Fresnel and Inverse-Fresnel 

### World-Space oriented coverage

### Box Projection



