# iscclosure

#### Function Characteristics

_This function is pure; it does not modify any data when called._

```lua
function iscclosure(closure: (...any) -> (...any)): boolean
```

#### High-Level Overview

Validates that the provided `closure` is implemented in C/C++.

#### Low-Level Overview

The `lua_iscfunction` Luau C function is used, and its return is pushed onto the Luau stack as the result of this function.

***

This function can be implemented as a debug.info/getinfo query. It can also be implemented as the negation of `islclosure`, as if something is a C closure, it cannot be a Luau closure, and viceversa.
