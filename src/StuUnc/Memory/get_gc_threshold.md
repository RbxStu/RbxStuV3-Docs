
# get\_gc\_threshold

#### Function Characteristics

_This function is pure; it does not modify any data when called._

```lua
function get_gc_threshold(iWantADamnDouble: boolean?): string
```

#### Remarks

This function returns a `string` due to the GC threshold being irrepresentable as integers (Luau is limited to 32-bit integers, while the GC threshold is a 64-bit integer). Because of it, a `string` representation of the GCthreshold is returned by default; however, by providing the `iWantADamnDouble` value as `true`, you will obtain it as a Luau double-precision floating point number. The latter is not the default. due to accuracy concerns.

The GC threshold, after collection, increases by the following math equation:

```cpp
// trigger cannot be correctly adjusted after a forced full GC.
// we will try to place it so that we can reach the goal based on
// the rate at which we run the GC relative to allocation rate
// and on amount of bytes we need to traverse in propagation stage.
// goal and stepmul are defined in percents
g->GCthreshold = g->totalbytes * (g->gcgoal * g->gcstepmul / 100 - 100) / g->gcstepmul;

```

In short, if the GC threshold is modified, it means that the Luau Garbage Collector collected some memory.

In the future, RbxStu V3 may allow access to more GC parameters; however, as of now it is not planned.

#### High-Level Overview

`get_gc_threshold` will return you the threshold before the Luau Garbage Collector runs again, useful for debugging memory usage. The return is a `string` by default; a number can be requested by passing in the optional `iWantADamnDouble` parameter as `true`.

#### Low-Level Overview

We validate the `iWantADambDouble` argument, after which we push into the luau stack the `L->global->GCthreshold` field, after which it is returned to the caller.