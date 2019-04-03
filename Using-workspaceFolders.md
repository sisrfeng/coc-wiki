Unlike VSCode，vim doesn't have workspace support. The solution is resolve workspace folder from opened files.

## Resolve workspace folder

A list of file/folder names is used for resolve workspace folder, the patterns could comes from three places:

* `b:coc_root_patterns` variable of current buffer.
* `rootPatterns` specified for language server used for current buffer.
* `"coc.preferences.rootPatterns"` settings, which default to `[".vim", ".git", ".hg", ".projections.json"]`.

The later one have lower priority, which means it's only used when previous patterns not exists or failed to match workspace folder.

To configure `rootPatterns` for specified filetype, use autocmd like:

``` vim
autocmd FileType python let b:coc_root_patterns = ['.git', '.env']
``` 

**Note** when the file is inside current vim's cwd, cwd is checked first. 

**Note** for performance reason, user's home directory would never considered as workspace folder.

**Note** since it works by resolve from files, to enable multiple workspace folders, you **have to** open at least one file of each folders.

## List current workspace folders

Use command `:CocCommand workspace.workspaceFolders` to get the list of current workspace folders.

Use command `:echo coc#util#root_patterns()` to get patterns used for resolve workspace folder of current buffer.

## Disable workspace folders support

You can add `"disableWorkspaceFolders": true` to the configuration section of the language server when you don't want the language server make use of workspaceFolders feature.

## Persist workspace folders

Variable `g:WorkspaceFolders` is used for store current workspace folders, use `set sessionoptions+=globals` in your vimrc if want to restore workspace folders from session.