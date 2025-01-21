
# delfolder

```lua
function delfolder(path: string): ()
```

#### Errors:

* _**Illegal Path**_
  * The path provided is considered 'unsafe'.
* failed to delete folder and all subfolders with error: "..."
  * Occurs when the folder or one of its subfolders cannot be deleted.
  * `...` will be the error message provided by the standard C++ filesystem library.&#x20;

#### Remarks

If the provided folder does not exist at `path`, then the function does nothing.

#### High-Level Overview:

Deletes the folder present at `path`, including all the files and subfolders it may contain.

#### Low-Level Overview:

`path` is read from the luau heap, then utilising `std::filesystem::is_directory` we validate if the provided path (once sanitised) is pointing to a valid directory in the filesystem; if so, it is deleted utilizing `std::filesystem::remove_all`.
