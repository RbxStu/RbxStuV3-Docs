# newlclosure

```lua
function newlclosure(closure: (...any) -> (...any)): (...any) -> (...any)
```

#### Remarks

This function will break if the environment of the emitted function is modified, do NOT call `setfenv` on the function that is returned, else it will not perform as expected.

#### High-Level Overview

Creates a Luau proxy that when called, will call `closure` with all the arguments provided to it.

#### Low-Level Overview

The function prepares two tables; as the function environment will be modified, the metatable contains an `__index` to another, it being the actual table we want to work on. In it we will push a global with an arbitrary name, after which we will compile Luau code that calls such a global. Then, when providing an env into `luau_load` we will provide the relative index of our environment table, after which the closure is returned.

When the function is called, it will first check its function environment for the function, and will later execute it. This is why calling `setfenv` on this function will destroy it, as no upreference or upvalue is used.
