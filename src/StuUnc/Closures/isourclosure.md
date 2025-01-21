
# isourclosure

#### Function Characteristics

_This function is pure; it does not modify any data when called._

```lua
function isourclosure(closure: (...any) -> (...any)): boolean
```

#### High-Level Overview

Validates that the provided `closure` is originating from the executor.

#### Low-Level Overview

The Luau compiler or the `luau_load` function are overridden to replace the line defined of every proto to `-1`. This allows us to validate if the Luau closure is originating from the executor.

For C closures, their `c.f` is checked; if it is within the `.text` section of ROBLOX, then it is not our function; if it isn't, however, then it is ours, as there is no way we have another DLL messing us up.

This makes checking the C closure's simple pointer mathematics and is much cheaper than keeping an entire function hashset or vector.
