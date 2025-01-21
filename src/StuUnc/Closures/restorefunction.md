# restorefunction

```lua
function restorefunction(closure: (...any) -> (...any)): ()
```

#### Errors

* _**Cannot obtain original function to unhook.**_
  * Caused when the original function is deleted or modified on the Luau Registry, resetting the hook would lead to danger in such cases; because of it, the function will not unhook the target `closure`.

#### High-Level Overview

Unhooks the `closure` if it has been previously hooked using `hookfunction`.;

#### Remarks

Functions that are not hooked will not error this function, unlike other implementations.

This function will revert cross-function hooks and functions that have been hooked multiple times; however, restoring a function hooked multiple times may have implicitly imposed memory leaks, as the previous hooks cannot be reversed, and the memory will still be live. Consider this before trying to unhook a deeply hooked function.;

#### Low-Level Overview

Internally an `std::map` is kept, with all hooked functions as the key and metadata on the hook as the value; the function is checked using the pure `contains` function, as using brackets to index will implicitly create the value and return the default.

Once it has been checked, if it is not hooked, it will return early. However, if it is hooked, the function will then start comparing the `hookType`, no hook where Lua is one end and C is the other, is possible other than with wrapping, which can be restored with ease by swapping the pointers of the current original by a copied, hidden original on the Luau Registry.

It is to note that copies of objects left on the Luau Registry do not get collected, thus incurring a memory cost. If the original function is modified, the function cannot be unhooked, as doing so could lead to dangerous memory corruption. Thus if the object is not a `Closure *` or it no longer exists on the Luau registry, the function will error.

However, if it does exist, the contents of it are swapped, and if the function is a wrapped C closure, the pointer to the wrapped closure is freed. and all information is reset to that of the `original`, first hook placed.

_Below is the data structure used to maintain metadata on the hooked functions._

```cpp
// ReferencedLuauObject is a lua_ref wrapper with type-checking.
template<typename T, ::lua_Type U>
struct ReferencedLuauObject {
    int luaRef;

    ReferencedLuauObject() { this->luaRef = LUA_REFNIL; }

    explicit ReferencedLuauObject(int ref) { this->luaRef = ref; }

    ReferencedLuauObject(lua_State *L, int idx) { this->luaRef = lua_ref(L, idx); }

    std::optional<T> GetReferencedObject(lua_State *L) {
        try {
            if (this->luaRef <= LUA_REFNIL)
                return {};

            lua_getref(L, this->luaRef);
            if (lua_type(L, -1) != U) {
                lua_pop(L, 1);
                return {};
            }

            const auto ptr = lua_topointer(L, -1);
            lua_pop(L, 1);
            return reinterpret_cast<T>(const_cast<void *>(ptr));
        } catch (const std::exception &ex) {
            RbxStuLog(RbxStu::LogType::Warning, RbxStu::Anonymous,
                      std::format("Invalid ref? Cxx exception: {}", ex.what()));
            return {};
        }
    }
};

enum class FunctionKind { NewCClosure, CClosure, LuauClosure };

struct HookInformation {
    ReferencedLuauObject<Closure *, ::lua_Type::LUA_TFUNCTION> original;
    ReferencedLuauObject<Closure *, ::lua_Type::LUA_TFUNCTION> hookedWith;

    FunctionKind dwHookedType;
    FunctionKind dwHookWithType;
};
```

Once the function is unhooked and all the pointers are swapped, the hook is removed from the map, and all the used resources are cleaned up.