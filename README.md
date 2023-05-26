# vimv
`vimv` implementation allowing swapping filenames, chain and cyclic renaming and error handling.

## Explained
Few cases can arise while renaming multiple files at once:
```
i)
* ----> * ----> * ... * ----> O

ii)
* ----> * ----> * ... * ----> X (delete file)

iii)
* ----> * ----> * ... * ----> *
^                             |
|                             |
-------------------------------
```
`*(1) ----> *(2)` means file `(1)` is renamed as file `(2)`.  
`*` denotes a file which is listed for renaming.  
`O` denotes a filename which is not listed for renaming. `O` might be a completely new filename or be the name of some file that already exists.  

Multiple combinations of all these cases might occur.  

In case (i), if `O` is an already present filename, then a prompt comes up asking whether the user wants to replace the already present file.  
If the user chooses 'y' or 'Y', the rename chain is executed and file `O` is replaced. Otherwise, the renaming chain is not executed.  
If `O` is a new filename, then rename chain is simply executed.

In case (ii), the last file is deleted and rename chain is executed.  
**NOTE**: Leave an empty line in place of filename of the file which is to be deleted. Don't remove the whole line, otherwise the script will give errors.  

In case (iii), cyclic renaming is executed.

## `vimv_backend`
`vimv_backend` is the main file which does all the renaming stuff.  
```
USAGE:
vimv_backend $current_names_file $new_names_file
```
`$current_names_file` is a text file containing the original filenames of the files which are to be renamed.  
`$new_names_file` is a text file containing the final filenames of the files which are to be renamed.  

## `vimv`
`vimv` is just a helper script which uses `vimv_backend` to rename files.  
`vimv <dir>` takes in a directory and lists all its files in `$EDITOR` for editing filenames and then hands over the `$current_names_file` and `$new_names_file` to `vimv_backend`.  
