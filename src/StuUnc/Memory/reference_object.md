# reference\_object

```lua
function reference_object(objectToReference: { [any]: any } | thread | userdata | (...any) -> (...any) | buffer | string): number
```

#### Errors:

* _**The object must be collectible to be referenced in the Luau Registry.**_
  * The provided object is not accepted by this function, as it is not a collectible object. Check the function remarks to understand what constitutes a "Collectible" object.

#### Remarks

This function will ONLY allow you to reference collectable types; as detailed in Luau, we utilise the `iscollectable` macro to check for this. As a result, the types that this function can return truly are those marked at the return; however, here is the list if you do not know how to read Luau type definitions...

* `LUA_TSTRING`: `string`
* `LUA_TTABLE`: `table`
* `LUA_TFUNCTION`: `function`
* `LUA_TTHREAD`: `thread`
* `LUA_TBUFFER`: `buffer`


This function may lead to memory leaks if misused; the cleanup is expected to be done by the user using `unreference_object`.

#### High-Level Overview

`reference_object` will reference the object on the Luau Registry, making it so it cannot be collected. Useful for reversing data structures, testing, or simply pinning something in memory.

#### Low-Level Overview

We make use of the `lua_ref` function, and reference the element at the top of the luau stack once we validate that it is a collectable type, after which we return the index into the Luau Registry it provides back to the caller. To allow the object to be freed, use `unreference_object`.