# getgc

```lua
function getgc(includeTables: boolean?):
    { { [any]: any } | thread | userdata | (...any) -> (...any) | buffer | string }
```

#### Remarks

This function will ONLY return you collectable types; as detailed in Luau, we utilise a modification of the `iscollectable` macro to check for this. As a result, the types that this function can return truly are those marked at the return; however, here is the list if you do not know how to read Luau type definitions...

* `LUA_TSTRING`: `string`
* `LUA_TTABLE`: `table`
* `LUA_TFUNCTION`: `function`
* `LUA_TTHREAD`: `thread`
* `LUA_TBUFFER`: `buffer`

#### High-Level Overview

`getgc` will return all objects present in memory that are collectable, You can opt into obtaining tables by passing `includeTables` as `true`; otherwise, the value is assumed to be false, and no tables will be included in the return.

#### Low-Level Overview

We create a temporal structure called `GCOContext` to hold the pointer to our lua state, whether tables will be included, and the number of items found.

```cpp
typedef struct {
    lua_State *L;
    bool bShouldIncludeTables;
    int dwItemsFound;
} GCOContext;
```

Once defined, it is initialised by value on the stack, and the Luau garbage collector is temporarily suspended by setting the collection threshold to `SIZE_MAX` (maximum value of an `unsigned long long`), saving the original value into a variable to set it back on once we visit all the garbage collector pages.

Then, utilising `luaM_visitgco`from `lmem.h`, we iterate through all of the GC pages, returning false, as we are not collecting/deleting any objects. We use our context and validate if any of the objects is arked as dead utilising the `isdead` macro, if so, we do not include it on the final table. Then we validate that the GC Object's type tag is within the bounds of `LUA_TSTRING`and `LUA_TPROTO`, as the values within such range are collectable, we as well check if it is a table, and if it is so, we check if we want the tables included, after which we use `luaC_threadbarrier`, as detailed on the `lapi.cpp`.

luaC_threadbarrier usage
```cpp
/*
 * Functions that push any collectable objects to the stack *should* call luaC_threadbarrier. Failure to do this can result
 * in stack references that point to dead objects since black threads don't get rescanned.
 */
```

before pushing each object onto our target thread's stack and setting them as an indexed field on our array. After all GC pages are iterated, we set the GC threshold back to how it originally was to allow the garbage collector to continue collecting, and we return the table left at the top of the luau stack containing all GC objects.

***

As an alternative API, there is `filtergc` however, it is currently not implemented on RbxStu V3 due to it requiring the hash of a function's bytecode.