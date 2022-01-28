# vimv
`vimv` implementation allowing swapping filenames, chain and cyclic renaming and robust error handling.

## Explained
Few cases can arise while renaming multiple files at once:
```
i)
* ----> * ----> * ... * ----> X

ii)
* ----> * ----> * ... * ----> * (delete file)

iii)
* ----> * ----> * ... * ----> *
^                             |
|                             |
-------------------------------
```
`*(1) ----> *(2)` means file `(1)` is renamed as file `(2)`.  
`*` denotes a file which is listed for renaming.  
`X` denotes a file which is not listed for renaming. `X` might be a completely new filename or might exist.  

Multiple combinations of all these cases might occur.  

In case (i), if `X` is an already present filename then a prompt comes up asking whether the user wants to replace the already present file.  
If the user chooses 'y' or 'Y', the rename chain is executed and file `X` is replaced with whatever replaces its name.  
Otherwise the renaming chain is not executed.  
If `X` is a new filename, then rename chain is simply executed.

In case (ii), the last file is deleted and rename chain is executed. Leave a blank space in place of filename of the file which is to be deleted.  
Don't delete the whole line, otherwise the script will give errors.

In case (iii), the cyclic renaming is executed. This is same as swapping filenames in case of only 2 files.

## `vimv_backend`
`vimv_backend` is the main file which does all the renaming stuff.  
USAGE:
```

vimv_backend current_names_file new_names_file
```
`current_names_file` is a text file containing the original filenames of the files which are meant to be renamed.  
`new_names_file` is a text file containing the final filenames of the files which are meant to be renamed.  

## `vimv`
`vimv` is just a helper script which uses `vimv_backend` to rename files.  
`vimv <dir>` takes in a directory and lists all its files in `$EDITOR` for editing names
and hands over the `current_names_file` and `new_names_file` to `vimv_backend`.  
