# islclosure

#### Function Characteristics

_This function is pure; it does not modify any data when called._

```lua
function islclosure(closure: (...any) -> (...any)): boolean
```

#### High-Level Overview

Validates that the provided `closure` is implemented in Luau.

#### Low-Level Overview

The `lua_iscfunction` Luau C function is used, and the inverse of the return of it (`!lua_iscfunction`) is pushed onto the Luau stack as the result of this function.

***

This function can be implemented as a debug.info/getinfo query. It can also be implemented as the negation of `iscclosure`, as if something is a C closure, it cannot be a Luau closure, and vice versa.
