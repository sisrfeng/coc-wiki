Unlike VSCode，vim doesn't have workspace support. The solution is resolve workspace folder from opened files.

## Resolve workspace folder

A list of file/folder names is used for resolve workspace folder, `b:coc_root_patterns` can be used for specify root patterns of current buffer, for example:

``` vim
autocmd FileType python let b:coc_root_patterns = ['.git', '.env']
``` 

When `b:coc_root_patterns` not exists, `"coc.preferences.rootPatterns"` is used, it's default to `[".vim", ".git", ".hg", ".projections.json"]`.  

Workspace folder is resolved from up to down, the resolved folder would never be user's home directory. 

**Important** since it works by resolve from files, to enable multiple workspace folders, you **have to** open at least one file of each folders.

## List current workspace folders

Use command `:CocCommand workspace.workspaceFolders` to get the list of current workspace folders.

Use command `:echo coc#util#root_patterns()` to get patterns used for resolve workspace folder of current buffer.

## Disable workspace folders support

You can add `"disableWorkspaceFolders": true` to the configuration section of the language server when you don't want the language server make use of workspaceFolders feature.