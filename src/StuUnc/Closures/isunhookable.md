
# isunhookable

#### Function Characteristics

_This function is pure; it does not modify any data when called._

```lua
function isunhookable(closure: (...any) -> (...any)): boolean
```

#### High-Level Overview

`isunhookable` returns `true` for every function that is considered unhookable; this means you cannot call `hookfunction` with it being `hookWhat`⁣; this does not limit you to calling `hookfunction` with it as `hookWith`. You can make functions unhookable by utilising `makeunhookable`.

#### Low-Level Overview

Internally, an `std::vector` of all the unhookable functions of the session are kept. When a new one is made unhookable, it is added to the vector.

When validating if it is hooked, we check our list and validate the `c.f` for C closures and `l.p` for Luau closures; if any of them match, the function is considered unhookable, the result of it being pushed onto the Luau stack.
