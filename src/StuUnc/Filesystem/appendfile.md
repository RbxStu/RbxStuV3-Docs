
# appendfile

```lua
function appendfile(path: string, content: string): ()
```

#### Errors:

* _**Illegal Path**_
  * The path provided is considered 'unsafe'.
  * The extension is considered unsafe.
* _**Failed to open file handle**_
  * Rare error. Occurs when the file cannot be opened to write to. This means ROBLOX Studio is lacking permission to open the file, or it is currently being opened exclusively by another process.



#### High-Level Overview:

The `appendfile` function follows the already defined UNC specification. It will try to write to the file at `path`, and append `content` to its existing content.

#### Low-Level Overview:

For each transaction on the filesystem library, we open a handle to the file targeted by the path parameter. The string is read from the Luau heap, and then the content is written using `std::ios::ate`. Which will set the position of the cursor to the end of the file without overwriting or modifying any contents inside of the file; then it is written to by appending it, the buffer is flushed, and the handle is closed.
