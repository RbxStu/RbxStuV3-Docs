# clonefunction

```lua
function clonefunction(closure: (...any) -> (...any)): (...any) -> (...any)
```

#### High-Level Overview

Creates a copy of `closure`, that even if `closure` is modified with, for example, `hookfunction`, has the same behaviour.

#### Low-Level Overview

Luau closures are trivially copyable, as in the Luau C API there already is a method to copy them. `lua_clonefunction` For C closures, we must copy all the members and appropriately copy all the upvalues; this includes modifying the internal `newcclosure` map to keep track of which function should call what, should it ever be cloned.
