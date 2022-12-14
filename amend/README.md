% Amend — Code Revision Tool

# Amend

Source code revision tool.

## Description

A lot of software is accompanied by additional developer tools to update or check
certain features of the source code. This may, for example, include updating a date,
such as in the copyright notice. Another example is the [Lua](https://www.lua.org) 
[source code](https://github.com/lua) itself, 
where you encounter [comments](https://github.com/lua/lua/blob/master/lopcodes.h), 
such as:

    ** Grep "ORDER OP" if you change these enums. Opcodes marked with a (*)
    ** has extra descriptions in the notes after the enumeration.

The author(s) of ''amend'' have also repeatedly reinvented wheels (read utilities)
to prepare software releases - for each new software they were working on.

FIXME
