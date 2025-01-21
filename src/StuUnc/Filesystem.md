# Filesystem

The filesystem library in RbxStu V3 can be accessed in a manner of two ways: globally (no prefix) or by its library name, `filesystem`. All filesystem functions are herein considered **EXPENSIVE** functions due to their nature.

#### Why?

The traditional UNC Filesystem forces the developer to, on every operation, open a handle to the file and then close it. Because of it, it is considered slow. All that can truly be done is to make a big buffer to then flush it into the file using appendfile or writefile to try and mitigate these effects.

***

The filesystem library works by creating a folder named **workspace** inside of the folder the DLL is present in; this means that, wherever you place the DLL, must be a place where ROBLOX Studio can write and read from; otherwise, it will fail to create the folder and, to a lesser extent, not work.

