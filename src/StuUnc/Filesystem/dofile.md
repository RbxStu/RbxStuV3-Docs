
# dofile

```lua
function dofile(path: string): () 
```

#### Remarks

The function internally uses `task.spawn` to schedule the thread; this function will inherit errors from `readfile`, as internally the function is reused for simplicity's sake.

#### High-Level Overview:

Reads the contents at the file present at `path`, then calls `loadfile` and schedules the function to run on a new thread created from the current one.

#### Low-Level Overview:

`path` is read from the luau heap, then `loadfile` is pushed onto the luau stack and it's arguments are prepared; once done, it will obtain `task.spawn` and call it, with the first argument being the function obtained from `loadfile`.
