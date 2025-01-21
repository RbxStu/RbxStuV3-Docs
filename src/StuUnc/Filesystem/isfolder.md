
# isfolder

#### Function Characteristics

_This function is pure; it does not modify any data when called._

```lua
function isfolder(path: string): boolean
```

#### Errors:

* _**Illegal Path**_
  * The path provided is considered 'unsafe'.

#### High-Level Overview:

The `isfolder` function follows the already defined UNC specification; it will check if the provided `path` points to a valid folder in the filesystem.

#### Low-Level Overview:

`path` is read from the luau heap, then utilising `std::filesystem::is_directory` we validate if the provided path (once sanitised) is pointing to a valid folder in the filesystem.
