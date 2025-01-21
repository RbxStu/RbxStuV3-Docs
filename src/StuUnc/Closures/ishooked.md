# ishooked

#### Function Characteristics

_This function is pure; it does not modify any data when called._

```lua
function ishooked(closure: (...any) -> (...any)): boolean
```

#### High-Level Overview

`ishooked` returns `true` for any function passed into it that has been hooked in this session.

#### Remarks

The function will return `false` if the function has been hooked but later restored using `restorefunction`.

#### Low-Level Overview

Internally an `std::map` is kept, with all hooked functions as the key and metadata on the hook as the value; the function is checked using the pure `contains` function, as using brackets to index will implicitly create the value and return the default.
