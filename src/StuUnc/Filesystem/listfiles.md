
# listfiles

```lua
function listfiles(path: string): { string }
```

#### Errors:

* _**Illegal Path**_
  * The path provided is considered 'unsafe'.

#### Remarks

Whilst it may seem that `listfiles` implies that only files will be enumerated, in reality, both files and folders will be enumerated.

#### High-Level Overview:

The `listfiles` function follows the already defined UNC specification; it will return all the folders found at the provided `path`.

#### Low-Level Overview:

`path` is read from the luau heap, then utilising `std::filesystem::is_directory` we validate if the provided path (once sanitised) is pointing to a valid folder in the filesystem; if it is, then we use an `std::filesystem::directory_iterator`. and push the paths of the files found to the array that is returned.
