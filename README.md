# lua-xattr

A library for getting and setting extended file attributes on Linux.

It makes the setxattr/getxattr/listxattr function family from libattr
available in Lua. 

# Building

## Requirements

* Lua 5.2 development files 

* libattr development files (we expect to have <attr/xattr.h> in our
  include path)

* pkg-config to query --cflags and --libs

## Build proces

```sh
make
```

# Documentation (by example)

```lua
local lx = require 'lxattr'

--- set an attribute on a file, dereferencing symlinks
-- @param path (non-empty string) relative or absolute file path
-- @param name (non-empty string) name of the attribute
-- @param value (string) value of the attribute
-- @return TRUE if successful; NIL if a parameter does not meet our
-- expecations; NIL and an INTEGER if a C function returned an error.
-- The integer is glibc's errno error code.
local bool, errno = lx.set(path, name, value)

--- set an attribute on a file, NOT following symlinks
local bool, errno = lx.lset(path, name, value)

--- list a file's attributes, following symlinks
-- @return attribute names (list of strings)
local table, errno = lx.list(path)

--- list a file's attributes, NOT following symlinks
-- @return like list()
local table, errno = lx.llist(path)

--- get an attribute $name from the file at $path
-- @param path (non-empty string) file path
-- @param name (non-empty string) name of the attribute
-- @return if successful a string which may contain binary data and
-- multiple \0s. May be an empty string. If getxattr() failed,
-- returns NIL and the failure's errno code. Returns NIL if the
-- arguments do not fit our expectations.
local binstring, errno = lx.get(path, name)
local binstring, errno = lx.lget(path, name)
```
