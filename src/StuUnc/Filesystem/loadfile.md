# loadfile

```lua
function loadfile(path: string, chunkName: string?): (() -> () | nil, string?)
```

#### Remarks

The function returned by `loadfile` inherits the capabilities/security of the last Luau function called before reaching into the C implementation of `loadfile`. This means that the loaded code will have the same security as the code that executed it. If the code requires higher permissions, then it will fail to execute successfully.

If `chunkName`is `nil` then it will default to `=RbxStuV3`.;

Once this function is called, environment access optimisations will be disabled. This means that access to globals may be slower, and `fastcall` for libraries like `math` will not be used. This will also happen when using `getfenv` or `setfenv` in normal Luau.

This function will inherit errors from `readfile`, as internally the function is reused for simplicity's sake.

#### High-Level Overview:

Reads the contents at the file present at `path`, compiles, and then loads the function using the provided `chunkName` for execution. If a compilation error arises, the first return of the function will be `nil`, and the second return will detail what occurred.

#### Low-Level Overview:

`path` is read from the luau heap, then utilising `std::filesystem::is_regular_file` we validate if the provided path (once sanitised) is pointing to a valid file in the filesystem; if so, it is read, then compiled into Luau Bytecode and loaded using the provided `chunkName`⁣. If compilation were to fail, the first argument is pushed as `nil`, and the error string is pushed on top of it; otherwise, the function will inherit the capabilities of the last luau function that was called by the `thread`.
