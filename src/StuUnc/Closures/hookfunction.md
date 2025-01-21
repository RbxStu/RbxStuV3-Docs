# hookfunction

```lua
function hookfunction(
        hookWhat: (...any) -> (...any),
        hookWith: (...any) -> (...any)
): (...any) -> (...any)
```

#### Errors

* _**This function cannot be hooked from Luau.**_
  * `hookWhat` is marked as unhookable.

#### High-Level Overview

Hooks `hookWhat` with `hookWith`, replacing it in memory, and returning `hookWhat` so it can be called on `hookWith` to detour back into the original code flow.

#### Remarks

This function cannot hook unhookable functions and will error if it is attempted to.

This function can hook a function more than one time; however, this is not particularly recommended as it leads to memory leaks. `hookWhat` will always be referenced once hooked, and even if a function is restored using restorefunction, the references to the hooks that are not the last one will remain on memory until the `DataModel` is deleted.

#### Low-Level Overview

Internally an `std::map` is kept, with all hooked functions as the key and metadata on the hook as the value; the function is checked using the pure `contains` function, as using brackets to index will implicitly create the value and return the default. If it is not there, it will create it using the defaults and call `clonefunction(hookWhat)` twice. One to return to the caller, and one to pocket and keep in memory to use with `restorefunction` if the user so desires to restore the function.

Once checked, `hookWith` is wrapped if required (for example, if `hookWith` was to have more `upvalues/upreferences` than `hookWhat`, or if they were of different closure types). Then the pointers are swapped, and `hookWhat` is referenced so that calling it does not cause any errors or crashes, as the values may appear to be completely unrelated for the Luau GC and collect `hookWith` without considering that it is now being used for `hookWhat`.

Once the hook is placed, it is saved into the map that keeps track of the hooked functions with the corresponding metadata required for unhooking, and the cloned function of `hookWhat` is returned.;
