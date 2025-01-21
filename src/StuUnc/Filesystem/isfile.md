
# isfile

#### Function Characteristics

_This function is pure; it does not modify any data when called._

```lua
function isfile(path: string): boolean
```

#### Errors:

* _**Illegal Path**_
  * The path provided is considered 'unsafe'.

#### High-Level Overview:

The `isfile` function follows the already defined UNC specification; it will check if the provided `path` points to a valid file in the filesystem.

#### Low-Level Overview:

`path` is read from the luau heap, then utilising `std::filesystem::is_regular_file` we validate if the provided path (once sanitised) is pointing to a valid file in the filesystem.
