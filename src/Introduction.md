# Introduction
Welcome to the RbxStu V3 documentation!

## What is RbxStu V3?

RbxStu V3 is a modification for ROBLOX Studio, it's focus is to improve the developer experience of users, providing them with tools that allow them to run code on the engine without restrictions. The modification mainly focuses on the Luau aspect of the engine. Luau is the fork of Lua 5.1 ROBLOX uses for their engine, and has many changes from the up-stream Lua 5.1, these changes include:

* Type Checking
* Native Code Generation
* Sandboxing
* Performance Improvements (Across the board in ALL aspects)

These are some of the changes present in Luau. The API for Luau on C is undocumented as it currently stands.

## What will the documentation contain?

This documentation will _not_ teach you how to write **Luau C API** C code, as we will refer to it from now on, we may sometimes make some references to internal functions present on the C API to explain _how_ some functions are implemented for developers to know what consequences it may have on the entire VM, as well as allow other potential alternatives to RbxStu V3 to implement them as well.

The documentation may also contain some ROBLOX Studio internal information, including but not limited to:

* Struct definitions (in C/C++ style)
* Pseudo Code extracted from Binary Ninja
* Implementation Information

This means the documentation may go (sometimes) in-depth, with graphical content to demonstrate some punctual things throughout ROBLOX Studio.

I re-iterate, this is _**not**_ meant as guide on how to make a ROBLOX Client hack on the likes of _**Synapse X**_ or _**Script-Ware**_, the ROBLOX Client and ROBLOX Studio are not equal in the slightest.

The ROBLOX Client has a lot of protections on the likes of Hyperion Anti-Tamper, Luau VM Shuffles and Luau VM Values (Sometimes referred to as **Encryptions** by exploit developers), this documentation will _only_ dwelve into any of the aformentioned if explicitly required, as they are not included in ROBLOX Studio.