# Connections
Connections in Roblox are, in general, a weird concept. They come in two forms.

`C Connections` and.`Luau Connections` Luau connections are connections formed using the `RBXScriptSignal`structure, while C connections are natively created using the C implementation of it, instead of its Luau abstraction.

To interact with connections, you can make use of the UNC-defined `getconnections`function.

```lua
function getconnections(signal: RBXScriptSignal): { RbxStuConnection }
```

#### High-Level Overview:

`getconnections`works by obtaining the connections that are part of the RBXScriptSignal's linked list and pushing a Luau representation of the native structures.

#### Low-Level Overview:

`getconnections` works by first obtaining a pointer to the head of the list. This is done by first establishing a connection itself using `:Connect` , obtaining an`RBXScriptConnection`, also known as a `SignalBridge` in some cases. 

This signal bridge holds pointers to the head of the list and to the 'owner,' being the signal itself. After obtaining the respective pointer, it is dereferenced over and over until we reach the end of the list, pushing an abstraction that will do the required modifications to the structure with each function.

#### Remarks

All connections are only being fired on the client. No connection is ever fired to the server side when using the Connections API. This is why functions like `replicatesignal` exist, as there is no way to replicate signals using the approach `getconnection` uses.

***

RbxStuConnection

This is an abstraction on top of the ROBLOX connection structure, it can be used to manage all the aspects of a Luau Connection, yet not all of a C connection.

#### Fields

* `Enabled: boolean`
  * If true, the connection is fired when the event is triggered, else, it is false.
* `Function: function?`
  * This field is `nil` on `C Connections` and on connections that were established in another Luau VM (Actor/different global state).
* `LuaConnection: boolean`
  * Whether the connection was established using C or Luau, resulting in false or true, respectively.
* `ForeignState: boolean`
  * Whether the connection was established in another Luau VM (Actor/different global state).
* `Thread: thread`
  * The thread that established the connection.
  * This field is `nil`on `C Connections`and on connections that were established in another Luau VM (Actor/different global state).;

#### Functions

* `RbxStuConnection:Fire(...) -> ()`
  * _Proxy to RbxStuConnection:Defer(...)_
* `RbxStuConnection:Defer(...) -> ()`
  * Fires the connection by utilising `task.defer` on a new thread created by the Roblox global state; this prevents potential `Environment Hijack` attempts.
  * Connections formed by another Luau VM will not be fired and will error on call.
  * Attempting to call `C Connections`will error due to security complications.
* `RbxStuConnection:Disable() -> ()`
  * This function will disable the connection from being fired; however, it will not disconnect it.
* `RbxStuConnection:Enable() -> ()`
  * This function will allow the connection to be fired when the event is triggered.
* `RbxStuConnection:Disconnect() -> ()`
  * This function will disconnect the connection by calling `RBXScriptConnection:Disconnect()` on it.
  * _**This allows the developer to know you have disconnected it**_; thus, unless you want to be destructive, it is heavily recommended to use `RbxStuConnection:Enable()`and `RbxStuConnection:Disable()`instead.

#### Remarks

While firing across Luau VMs is planned to be supported, it is currently _not_ included on the alpha release of RbxStu V3 due to security concerns with memory corruptions by potentially passing GCables across the LVM boundary, which could at any moment be freed by the other Luau VM, resulting in dangerous conditions.

#### Low-Level Overview

The `Function`and`Thread`fields are referenced on the `Luau Registry`. This means you can grab the function from the field and search for it in the `Lua Registry`. This also means you could replace it yourself using `hookfunction`, creating `hookconnection` if you will; this also means (most likely) that if a thread or function is present on the,`Luau Registry` then it is likely it is the result of an RBXScriptSignal connection.



#### Examples

_Obtaining all connections of an RBXScriptSignal_

```lua
local connectionsList = getconnections(game.ChildAdded) -- Obtains the list of connections that game.ChildAdded has established

for _, connection in connectionsList do
    print(connection.Enabled)        -- Print if the connection is Enabled
end

```

_Disabling all connections of an RBXScriptSignal_

<pre class="language-lua"><code class="lang-lua">local connectionsList = getconnections(game.ChildAdded) -- Obtains the list of connections that game.ChildAdded has established

for _, connection in connectionsList do
    print(connection.Enabled)        -- Should print true
    connection:Disable()             -- Disable the connection.
    print(connection.Enabled)        -- Should print false and no longer fire the connection.
end
</code></pre>

_Disconnecting all connections of an RBXScriptSignal_

```lua
local rbxConnection = game.ChildAdded:Connect(function() end)
local connectionsList = getconnections(game.ChildAdded) -- Obtains the list of connections that game.ChildAdded has established

for _, connection in connectionsList do
    print(connection.Enabled)        -- Should print true
    connection:Disconnect()          -- Disconnect the connection.
    print(connection.Enabled)        -- Should print false.
end
print(rbxConnection.Connected)    -- Should print false
```

***

There is no alternative API to the Connections API, and there are no plans to include `replicatesignal`due to its potential complexity.