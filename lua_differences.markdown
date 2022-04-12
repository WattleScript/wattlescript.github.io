---
layout: default
title: Differences between WattleScript Lua and Lua
---

# Differences between WattleScript Lua and Lua

WattleScript's Lua mode contains minor differences and a few additions, inherited from MoonSharp.

### List of Differences

* Strings are Unicode: some caution is required if code uses strings to store binary data. On the other hand, strings used as real strings are a lot easier to use.
* Garbage Collection is different, as WattleScript makes use of the .NET GC. Notably this makes `collectgarbage()` a no-op.
* Compatibility is source-only, WattleScript cannot load luac binaries.
* Slight differences in error messages.
* Function names are tracked at function declaration, not backtracked at function calls like in Lua (noticeable only when debugging)

### List of Additions

* Multiple expressions can be used as indices but the value to be indexed must be a userdata, or a table resolving to userdata through the metatable (but without using metamethods). So, for example, ``x[1,2,i]`` is parsed correctly but might raise an error if ``x`` is not a userdata.
* Metalua short anonymous functions (lambda-style) are supported. So ``|x, y| x + y`` is shorthand for ``function(x,y) return x+y end``. 
* A non-yieldable ``__iterator`` metamethod has been added. It's called if the argument ``f`` of a ``for ... in ... `` loop is not actually a function.
* A default iterator is provided if the argument ``f`` of a ``for ... in ...`` loop is a table without ``__iterator`` or ``__call`` metamethods. The provided result is `value, key` unlike `ipairs` or `pairs`.
* ``\u{xxx}`` escapes, where x are up to 8 hexadecimal digits, are supported inside strings and will output the specified Unicode codepoint, as it does in Lua 5.3
* ``loadsafe`` and ``loadfilesafe`` methods, same as ``load`` and ``loadfile`` but defaulting to the current top-of-the-stack ``_ENV`` instead of the default one, for easier sandboxing
* The ``dynamic.eval`` and ``dynamic.prepare`` functions to handle dynamic expressions
* ``string.unicode`` method, just like ``string.byte`` but returning a whole unicode codepoint
* ``string.contains``, ``string.startsWith`` and ``string.endsWith`` methods allow easier and faster performing string ops without having to rely on patterns for simple scenarios
* The ``json`` module to support JSON<->table conversions

