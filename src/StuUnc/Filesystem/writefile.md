
# writefile

```lua
function writefile(path: string, content: string): ()
```

#### Errors:

* _**Illegal Path**_
  * The path provided is considered 'unsafe'.
  * The extension is considered unsafe.
* _**Failed to open file handle**_
  * Rare error. Occurs when the file cannot be opened to write to. This means ROBLOX Studio is lacking permission to open the file, or it is currently being opened exclusively by another process.

#### High-Level Overview:

The writefile function follows the already defined UNC specification. It will create or open a file at `path` and overwrite its contents with `content`.

#### Low-Level Overview:

For each transaction on the filesystem library, we open a handle to the file targeted by `path`. The `content` string is read from the Luau heap, and then `content` is written to the file using `std::ios::trunc`. Which will set the position of the cursor to the beginning of the file, regardless of anything, and truncate all contents; then it is written to, the buffer is flushed, and the handle is closed.
