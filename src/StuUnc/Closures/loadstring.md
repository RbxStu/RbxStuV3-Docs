# loadstring

```lua
function loadstring(source: string, chunkName: string?): ((...any) -> (...any) | nil, string?)
```

#### Remarks

If no `chunkName` is provided, `=loadstring` will be used in its place.

The proto inherits the capabilities from its latest caller in Luau context.

`source`is compiled with optimisation level 1 and debugging level 2.

This function as a side effect, will disable environment optimisations by declaring the current thread's environment to be unsafe; this means that code may execute slower in Luau due to some optimisations of the LVM not being available anymore.

#### High-Level Overview

Compiles the provided `source` into a Luau closure that can be run.

#### Low-Level Overview

The source and chunkName are obtained from the Luau stack and are compiled utilising the `Luau::Compile` function after source is compiled into Luau Bytecode; it is then loaded utilising luau\_load, with the environment being the global environment. Should compilation or loading the closure fail, the first return will be nil, whilst the second will be the error. Should it succeed, it will return the closure as its first return.

