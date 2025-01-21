# newcclosure

```lua
function newcclosure(closure: (...any) -> (...any)): (...any) -> (...any)
```

#### Remarks

This function can error when calling the C proxy as a result of modifications to the environment or attempts at hijacking the closure.

#### High-Level Overview

Creates a C proxy that when called, will call `closure` with all the arguments provided to it.

#### Low-Level Overview

RbxStu V3 keeps a map of `Closure *` to `int`, where int is a reference to the actual backing closure of the C proxy. The call that the proxy calls onto can be modified; however, if this ever happens, then it would not result in any crashes, as V3 prevents resolving calls if a problem such as, say, replacing the Closure with a Table in the Luau Registry were to occur.

The C proxy is pushed, the continuation and the C function. The continuation is used to permit yielding.

#### How?

C closures can yield, given that their continuation handles the exception thrown; this is why functions like `ypcall` this used to exist, since this wasn't possible until they decided to add it and deprecated that function in favour of the now standard `pcall`, which can also yield and is in fact a C proxy as well and is what we are replicating in our `newcclosure` in fact. Most of the `newcclosure` basis can be obtained from it, except that our function comes from somewhere different, either an upvalue or from an internal Luau Registry reference map.

As if we ever yield, what will occur is that our thread will hit the scheduler. Once it is there and the work is done, which would signal Luau to resume, it will call the `resume` C function; being that in reality it isn't yieldable, the code will begin trying to search for a handler, marked in the `CallInfo` by `LUA_CALLINFO_HANDLE` in the `CallInfo` flags, this can be seen in the `resume_findhandler` function, in `resume_handle`it can be seen how it will call the continuation to handle our exception rooted from being unable to yield; however, it will be able to once the call completes, as it will be able to proceed thanks to our continuation.

The C proxy obtains from the map the original reference and fetches it from the Luau Registry; it validates it, and once it's complete, it will push all the arguments, the function itself, and execute it`lua_pcall` with all the things previously discussed; should we error, our continuation will catch it.&#x20;

However, there is an additional step when handling errors aside from yielding. Our errors are coming from Luau; this means they will contain the chunk name and line number where the error originates. This makes the error not be one originating from C, and if you were to error inside of a C closure, you would leak this information to the caller, and in hooks this means you will be detected.

RbxStu V3 will read the error, and using the following regex, it will remove the chunkname and line number from the error, providing a C-like error, `.:(\d):`