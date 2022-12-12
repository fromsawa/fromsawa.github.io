# Amend

Source code revision tool.

## Description

FIXME

## API

### Globals

     EXECDIR             Path, where ''amend.lua'' was started.
     ROOTDIR             Project root (where ''.amend/project.lua'' was found).
     PROJECTFILE         Full path to the ''.amend/project.lua'' file.
     PROJECT             Project settings.
     CONFIG              Amend settings.
     TOOLS               System tools (see 'amend/tools.lua')
     PATHS               Additional module paths.
     IGNORE              Files or directories generally to ignore (list of regex patterns, using .gitingore if available).
     VERBOSE             Verbosity level.

### Project configuration


FIXME


### Components


"Amend components" can be created anywhere in the source code tree in a sub-folder ".amend".
The first line of such a component starts with a shebang of the format


    #![indicator]<component-name> [<argument-description>] -- <component-description>


where the 'indicator' is

    *           The component is always executed (even if option 'all' is not used).
    _           This defines a "hidden" component (which is only executed as dependency).

If the <indicator> is omitted, the component behaves like a command and is executed only on 
explicit request.

Example:


    #!*check [<arguments>] Check system.

    depends "check-os"
    verbose "Checking system"

    assert(os.command("cmake --version"))

...
    

#### `depends '<name>'`

Include a dependecy.

#### `message [{<level>}] "<text>"`


#### `message(<level>|<level-name>, "<text>", ...)`

Emit an informational message.


**Arguments:**

FIXME


### Utilities


#### Editing


Amend provides several utilities for editing files.


#### **section**:`clear()`

Clear contents.

#### **section**:`addln(code, ...)`

Add a code line.

#### **section**:`add(code, ...)`

Add code to current line.

#### **section**:`sed(pattern, replace)`

In-place sed.


#### **section**:`write(stream)`

Write section to a stream.

#### **section**`()`

Constructor.

#### **file**:`parse(path)`

Parse file (into sections)


#### `FIXME`

Update file.


#### `FIXME`

In-place sed.


#### `FIXME`

Constructor.

#### `FIXME`

Edit a file.
@fn edit.file(<filen-ame(s)>)

FIXME


#### Extensions to LuaFileSystem.


##### `fs.concat(...)`

Concatenate path elements.

**Arguments:**

     ...             List of path elements.

**Returns:** 
     Concatenated path elements using builtin directory seperator.


##### `fs.parts(fname)`

Get parts of a file name (path, file and extension).

**Arguments:**

     fname           The file- or path-name.

**Returns:**  <path>,<file-name>,<extension>


##### `fs.relpath(path, root)`

Get relative path with respect to a "root".

**Arguments:**

     path            The 'path' to split.
     root [optional] The root path.

**Returns:**  <relative-path>


##### `fs.readwild(file, tbl)`

Read a wildcard pattern file.

**Arguments:**

     file                The file name (containing wildcard patterns and comments).
     tbl [optional]      Existing table (with regex patterns).

##### `fs.dodir(path, callback, options)`

Execute a function for each directory element possibly recursively.

**Arguments:**

     path                    The path to iterate over.
     callback                Callback function for elements (must return true for recursion).
     options [optional]      Options.

This function executes the `callback` for each element in the alpha-numerically
sorted directory list. Arguments passed to the callback are:
     [1]     Table with entries as returned by `fs.parts` and the absolute path at index 0.
             The table also contains the key 'attr' with file attributes as returned by `symlinkattributes`,
             and 'options', with the `options` table.
If recursion is enabled, FIXME

Options:
     exclude             List of regex-patterns of files or directories to ignore (default: {'[.]', '[.][.]'}).
     include             List of regex-patterns of files or directories to include (overrides 'exclude').
     extension           Only report files or directories matching list of given extensions.
     mode                File type (`mode` field of `attributes()` function).
     follow              Follow symbolic links (default: false)
     recurse             Enable directory recursion (default: false).
     depth               Directory depth (default: 0)


##### `fs.pushd(dir)`

"Push" directory.

Equivalent of shell command ''pushd''.


##### `fs.popd()`

"Pop" directory.

Equivalent of shell command ''popd''.


##### `fs.rmkdir(fpath)`

Recursively create directory.

**Arguments:**

     fpath       The directory-path to create.

**Returns:**  <status>[, <error-message>]
     The value of LuaFilesystem's [mkdir](https://keplerproject.github.io/luafilesystem/manual.html#mkdir) command.


##### `fs.grep(fname, pattern)`

Grep-like matching


##### `fs.filetype(fname)`

Get file type (from extension).
FIXME

#### CSV-file tools.

`csv.load(fname, [opts])`
 Read a CSV file.

**Arguments:**
 
     fname       File name.
     opts        Options.

Options:
>    {
>        comment = '<pattern>',
>        separator = 'separator',
>        columns = { <column-names-list> },   
>        filter = <item-filter-function>
>    }


#### YAML support.


Here, the official [YAML bindings](https://github.com/gvvaughan/lyaml) are used
with some extensions.

FIXME tbd


### Lua extensions


#### Package


##### `package:addpath(path)`

Add search directory to script path.

##### `package:pushpath(path)`

Temporarily add a script search path.

##### `package:poppath()`

Remove previously added script search path.

#### String


##### `string.any(s, tbl, exact)`

Match elements from a table.


**Arguments:**

     s                   The string.
     tbl                 Table with regex-patterns.
     exact [optional]    Boolean value indicating if matching must be exact.


**Returns:** 
     Matched string, otherwise ''nil''.


##### `string.trim(s)`

Trim string.

##### `string.title(s)`

Make string "titlecase".

##### `string.untitle(s)`

Undo "titlecase".

##### `string:split(sSeparator, nMax, bRegexp)`

String split.
See http://lua-users.org/wiki/SplitJoin

#### Table


##### `table.has(tbl, item)`

Check if array-part of a table has an element.

**Arguments:**

     tbl         The table to check.
     item        The item.


##### `table.kpairs(tbl)`

Key-only table iterator.
This function ignores integer-valued keys.

##### `table.count(tbl)`

Count all keys in a table.

##### `table.hint(tbl)`

Get/set "hint" table.
FIXME

##### `table.top(tbl, idx)`

Get array items from top

##### `table.copy(tbl)`

Create a table copy.

#### IO


##### `io.printf(...)`

Equivalent to C's ''printf''

##### `io.dump(value, options)`

Dump value.

**Arguments:**

     value                   Value to stream to output.
     options [optional]      Display options.

This function dumps value to an output stream.

The ''options'' is a table, that may contain the following fields:

     stream          Output stream (default: io.stdout).
     indent          Indentation string.
     level           Indentation level.
     key             Table key.
     prefix          Prefix for output (usually only for adding a "return" statement).
     quoted          Always output keys in the ["quoted"] format (default: false)


##### `io.readall(fname)`

Read file.

**Arguments:**

     fname           The file name.

**Returns:**  text, error

##### `io.command(program, ...)`

Execute command and read output.

**Arguments:**

         program                 The command to execute (as format string).
         ...                     Format options.

**Returns:**  output,error
     FIXME

#### OS


##### `os.command(program, ...)`

Execute a command.

## License

    Copyright (C) 2021-2022 Yogev Sawa

    UNLICENSE

    This is free and unencumbered software released into the public domain.
    
    Anyone is free to copy, modify, publish, use, compile, sell, or
    distribute this software, either in source code form or as a compiled
    binary, for any purpose, commercial or non-commercial, and by any
    means.

    In jurisdictions that recognize copyright laws, the author or authors
    of this software dedicate any and all copyright interest in the
    software to the public domain. We make this dedication for the benefit
    of the public at large and to the detriment of our heirs and
    successors. We intend this dedication to be an overt act of
    relinquishment in perpetuity of all present and future rights to this
    software under copyright law.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
    EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
    MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
    IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
    OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
    ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
    OTHER DEALINGS IN THE SOFTWARE.

    For more information, please refer to <http://unlicense.org/>
