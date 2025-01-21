
# makefolder

```lua
function makefolder(path: string): ()
```

#### Errors:

* _**Illegal Path**_
  * The path provided is considered 'unsafe'.

#### Remarks

If the provided path is nested inside of non-existing folders, `makefolder` will create all the folders up to `path`.

#### High-Level Overview:

Creates a folder at `path`.

#### Low-Level Overview:

`path` is read from the luau heap, then utilising `std::filesystem::is_directory` we validate if the provided path (once sanitised) is pointing to a valid folder in the filesystem; if it is not, we create it using `std::filesystem::create_directories`, which allows for nested directories, even if they've not been created previously.
