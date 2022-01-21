Unlike VSCode vim doesn't have workspace support. The solution is resolve workspace folders from opened files.

## Resolve workspace folder

A list of file/folder names is used for resolve workspace folder, the patterns could comes from:

* `b:coc_root_patterns` variable of current buffer.
* `rootPatterns` specified for language server used for current buffer.
* `"coc.preferences.rootPatterns"` settings, which default to `[".git", ".hg", ".projections.json"]`.


The later one have lower priority, which means it's only used when previous patterns failed to match workspace folder.

When current cwd contains any of the patterns, it's used as workspace folder of the file, to disable this behavior, use configuration `workspace.workspaceFolderCheckCwd`.

Workspace folders are resolved from top to bottom, to change this behavior, use configuration `workspace.bottomUpFiletypes`.

When resolve of workspace folder failed, cwd is used for workspaceFolder, use `"workspace.workspaceFolderFallbackCwd": false` to avoid this behavior.

To configure `rootPatterns` for specified filetype, use autocmd like:

``` vim
autocmd FileType python let b:coc_root_patterns = ['.git', '.env']
``` 

**Note** for performance reason, user's home directory would never considered as workspace folder.

**Note** since it works by resolve from files, to enable multiple workspace folders, you **have to** open at least one file of each folders.

## Manage workspace folders

Use command `:CocList folders` to open list of workspace folders, `delete` and `edit` actions are supported.

Use command `:echo coc#util#root_patterns()` to get patterns used for resolve workspace folder of current buffer.

## Persist workspace folders

You can make use of vim's session for persist workspace folders.

As global variable `g:WorkspaceFolders` is used to store workspace folders, you need add `set sessionoptions+=globals` to your vimrc if you want to restore workspace folders when session loaded.
