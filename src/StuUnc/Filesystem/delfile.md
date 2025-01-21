
# delfile

```lua
function delfile(path: string): ()
```

#### Errors:

* _**Illegal Path**_
  * The path provided is considered 'unsafe'.
* failed to delete file with error: "..."
  * Occurs when the file cannot be deleted.
  * `...` will be the error message provided by the standard C++ filesystem library.&#x20;

#### Remarks

If the provided file does not exist at `path`, then the function does nothing.

#### High-Level Overview:

Deletes the file present at `path`.

#### Low-Level Overview:

`path` is read from the luau heap, then utilising `std::filesystem::is_regular_file` we validate if the provided path (once sanitised) is pointing to a valid file in the filesystem; if so, it is deleted utilizing `std::filesystem::remove`
