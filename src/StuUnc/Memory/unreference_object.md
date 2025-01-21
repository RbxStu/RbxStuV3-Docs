# unreference\_object

```lua
function unreference_object(ref: number): ()
```

#### Remarks

Utilising this function to unreference objects that do not belong to you or have not been created using `reference_object` may lead to errors. Do not attempt to unreference any functions, threads, etc., that are NOT your responsibility that are present on the Luau Registry, as they may be a connection or something significant, and rewriting them may potentially lead to undefined behaviour from Roblox itself, as it potentially NEVER expected something similar to happen.

The `ref` argument is an integer, not a floating point number.

#### High-Level Overview

`unreference_object` will unreference the provided index in the Luau Registry, allowing the object that was referenced there to be collected once again.

#### Low-Level Overview

We make use of the `lua_ref` function, and reference the element at the top of the luau stack once we validate that it is a collectable type, after which we return the index into the Luau Registry it provides back to the caller. To allow the object to be freed, use `unreference_object`.
