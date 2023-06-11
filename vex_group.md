https://www.sidefx.com/docs/houdini/nodes/sop/groupcreate.html\
https://www.sidefx.com/docs/houdini/vex/functions/setpointattrib.html\
```python
setpointattrib(0, "abc", @ptnum, 100);
setpointgroup(0, "ptgroup", @ptnum, 1) //create group
setpointgroup(0, "ptgroup", @ptnum, 0) // remove pt from group
setpointgroup(0, "ptgroup", @primnum, 0) // remove prim from group
i@insphere = inprimgroup(0, "sphere", @primnum) // return 1 if input primnum is in sphere group.
i@inbox = inprimgroup(0, "box", @primnum)*100
i@numfaces =  nprimitivesgroup(0, "sphere") // check out the number of face in the sphere group.
i[]@prims = expandprimgroup(0, "box") // return prim number as a array in the box group.
i@npt = nearpoint(1, "box", @P); // finding closest point in "box" group(group filter) on second input from 0 input(pt)
@P = @P + set(inpointgroup(0, "@P.x>0", @ptnum),0,0); // move along the vector by using group filter.
```
# setpointattrib
Sets a point attribute in a geometry.

If you don’t know the attribute class ahead of time, use [setattrib](https://www.sidefx.com/docs/houdini/vex/functions/setattrib.html).
```python
int  setpointattrib(int geohandle, string name, int point_num, <type>value, string mode="set")

int  setpointattrib(int geohandle, string name, int point_num, <type>value[], string mode="set")
```
Returns the value of geohandle on success or -1 on failure.

>Note
>If the attribute does not exist, this function creates the attribute with a default value of zero, empty string, or an empty array. If you >want to control the default value of a numeric attribute, use [addattrib](https://www.sidefx.com/docs/houdini/vex/functions/addattrib.html) >before setting the attribute.

Show/hide arguments
geohandle

A handle to the geometry to write to. Currently the only valid value is 0 or [geoself](https://www.sidefx.com/docs/houdini/vex/functions/geoself.html), which means the current geometry in a node. (This argument may be used in the future to allow writing to other geometries.)

name

The attribute to set on the given point.

point_num

The number of the point to set the attribute on.

value

The value to set the attribute to.

Note that within a VEX program only one type may be written to a single attribute. Ie, you cannot mix writes of float an integer. This can be surprising as a literal like 1 will be an integer write so be ignored if floats were previously written.

mode

(Optional) if given, this controls how the function modifies any existing value in the attribute.

"set"

Overwrite the attribute with the given value.

"add"

Add to the attribute the value.

"min", "minimum"

Set the attribute to the minimum of itself and the value.

"max", "maximum"

Set the attribute to the maximum of itself and the value.

"mult", "multiply"

Multiply the attribute by the value. For matrices, this will do matrix multiplication. For vectors, component-wise.

"toggle"

Toggles the attribute, independent of the source value. Useful for toggling group membership.

"append"

Valid for string and array attributes. Appends the source value to the end of the original value.

# setpointgroup
_Adds or removes a point to/from a group in a geometry._

`int  setpointgroup(int geohandle, string name, int point_num, int value, string mode="set")`

Show/hide arguments
geohandle

A handle to the geometry to write to. Currently the only valid value is 0 or [geoself](https://www.sidefx.com/docs/houdini/vex/functions/geoself.html), which means the current geometry in a node. (This argument may be used in the future to allow writing to other geometries.)

name

The name of the group to modify.

point_num

The point number to add or remove from the group.

value

1 to put the point in the group, 0 to remove the point from the group. This is ignored if mode is "toggle".

mode

Use "set" to set the point’s membership according to the value. Use "toggle" to toggle the point’s membership, regardless of the value.

# inprimgroup
_Returns 1 if the primitive specified by the primitive number is in the group specified by the string._
`int  inprimgroup(<geometry>geometry, string groupname, int primnum)`

Show/hide arguments
<geometry>

When running in the context of a node (such as a wrangle SOP), this argument can be an integer representing the input number (starting at 0) to read the geometry from.

Alternatively, the argument can be a string specifying a geometry file (for example, a .bgeo) to read from. When running inside Houdini, this can be an op:/path/to/sop reference.

Returns

1 if the groups exists and the primitive is in the group, or 0 otherwise.

This can use ad-hoc groups, like 0-3 or @Cd.x>0.5. It matches the SOP group naming convention, in particular that an empty string means all primitives.

# nprimitivesgroup
_Returns the number of primitives in the group._

`int  nprimitivesgroup(<geometry>geometry, string groupname)`

Show/hide arguments
<geometry>

When running in the context of a node (such as a wrangle SOP), this argument can be an integer representing the input number (starting at 0) to read the geometry from.

Alternatively, the argument can be a string specifying a geometry file (for example, a .bgeo) to read from. When running inside Houdini, this can be an op:/path/to/sop reference.

groupname

This must refer to an exact group name, not an adhoc group pattern.


# expandprimgroup
_Returns an array of prim numbers corresponding to a group string._
```
int [] expandprimgroup(<geometry>geometry, string groupname)
int [] expandprimgroup(<geometry>geometry, string groupname, string mode)
```
groupname can use ad-hoc groups, like 0-3 or @Cd.x>0.5. This uses the SOP group naming convention, in particular that an empty string means all.

mode can be ordered, unordered or split. ordered is the default mode and will return numbers in the order of appearance in the string, but only for numbers. The order won’t be kept when using expressions such as @Cd.x>0.5. The same number won’t appear twice in returned array. unordered mode returns the resolved group following sorted point numbers order. split mode starts by splitting the groupname string on @ characters and then does one resolution per sub string. The order is kept between the sub strings, but will fallback to unordered when resolving a group expression. This same number can appear multiple time when resolving using this mode.

# nearpoint
_Finds the closest point in a geometry._
```
int  nearpoint(<geometry>geometry, vector pt)
int  nearpoint(<geometry>geometry, vector pt, float maxdist)
int  nearpoint(<geometry>geometry, string ptgroup, vector pt)
int  nearpoint(<geometry>geometry, string ptgroup, vector pt, float maxdist)
```
Returns the number of the closest point on the geometry. This will only search against points, not the surface locations of the geometry. Use [xyzdist](https://www.sidefx.com/docs/houdini/vex/functions/xyzdist.html) to find the closest point on surfaces or curves.

-1 is returned if no point is found in the search distance.

Show/hide arguments
<geometry>

When running in the context of a node (such as a wrangle SOP), this argument can be an integer representing the input number (starting at 0) to read the geometry from.

Alternatively, the argument can be a string specifying a geometry file (for example, a .bgeo) to read from. When running inside Houdini, this can be an op:/path/to/sop reference.

ptgroup

A point group pattern to limit the search to. Can be a SOP-style group pattern such as 0-10 or @Cd.x>0.5. An empty string will match all points.

pt

The position in space to find the closest point on the geometry to.

maxdist

The maximum distance to search. The operation can be sped up if it is allowed to quit early.
