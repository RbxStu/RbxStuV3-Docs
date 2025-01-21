# hookmetamethod

```lua
function hookmetamethod(
        object: any,
        metamethod: string,
        hookWith: (...any) -> (...any)
): (...any) -> (...any)
```

#### Errors

* _**This function cannot be hooked from Luau.**_
  * _inherited from `hookfunction`_
* _**invalid metafield '...'**_
  * This error occurs when the provided `metamethod` is not valid.
* _**the metafield in the objects' metatable is not a function, and thus, cannot be hooked, use `getrawmetatable` and override it instead.**_
  * _Read remarks._
* _**cannot `hookmetamethod` on an object with no metatable, or that its metafield does not exist.**_
  * The provided `metamethod` is not present on the metatable of `object`.
* _**object has no metatable.**_
  * The provided `object` has no metatable.

#### High-Level Overview

Utilises `hookfunction` to hook the metamethod `metamethod` of  `object` with `hookWith`.

This function does not have an argument guard.

#### Remarks

_Inherits remarks from `hookfunction`_

This function will temporarily set the metatable to be readable and writable and will ignore the `__metatable` metafield.;

This function cannot hook metamethods `__index` or `__newindex` if they're being used as proxies into another table, as they're not functions.

As the name implies, this function hooks, it does not create a metamethod. If the metamethod does not exist, it will error.

#### Low-Level Overview

The metatable of the object is obtained utilising getrawmetatable, then the metafield is validated utilising the `luaT_eventname` string array. Once that is complete, the metafield is validated, and if it is not present, hooking will not proceed.

Once it is complete, `hookfunction` is utilised, and the metamethod is hooked with `hookWith`.
