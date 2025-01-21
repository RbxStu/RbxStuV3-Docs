# makeunhookable

```lua
function makeunhookable(closure: (...any) -> (...any)): ()
```

#### High-Level Overview

`makeunhookable` makes the passed function an unhookable function. This prevents it as being given to `hookfunction` as `hookWhat`, It does not limit it, however, and it can be used as `hookWith`.&#x20;

#### Low-Level Overview

Internally, an `std::vector` of all the unhookable functions of the session are kept. When the function is called, if the function is not already unhookable, it is pushed into the `std::vector`.
