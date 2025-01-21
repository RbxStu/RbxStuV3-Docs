
# readfile

```lua
function readfile(path: string, readAsBuffer: boolean?): string | buffer
```

#### Errors:

* _**Illegal Path**_
  * The path provided is considered 'unsafe'.
* _**This file doesn't exist**_
  * The path provided does not lead to a file inside of the **workspace**.
* _**File content too big**_
  * Rare error. Occurs when the file exceeds the fixed string or buffer limit, depending on the selected kind of read.

#### Remarks

Due to the way that the implementation is written, when the `readAsBuffer` parameter is specified, the file is read in `std::ios::binary` mode.
While when the argument is not specified, it will not be using this mode, this results in files that contain null bytes throughout and not only in the end to not be read to completion.

This is intended behaviour, and if you're dealing with binary data, you should look into using a buffer. Should you even require a string from such data for whatever the case may be, the `buffer.tostring(buf: buffer): string` method available on the buffer library should suffice you to get the string back.

#### High-Level Overview:

The `readfile` function follows the already defined UNC specification; however, there have been slight modifications to accommodate for more modern APIs and for working with binary file formats.

The "modification" in question is the ability to make readfile return a buffer or a string, depending on what the user selects by its argument.

#### Low-Level Overview:

For each transaction on the filesystem library, we open a handle to the file targeted by the path parameter. After which we validate that it does not exceed the fixed `string` and `buffer` size limits (depending on which is being used, albeit they're both limited to 1 GB in size) by using the `std::filesystem::file_size`function.

The file is then read to completion using an `std::istreambuf_iterator` into an `std::string` before pushing the string or buffer into the luau heap.

This means there will be an approximate _double_ memory usage for each time the file is read, as it is first copied to an `std::string`(C++ construct) before being transferred to a Luau GC object.
