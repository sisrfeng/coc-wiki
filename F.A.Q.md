Here's some common problems that you may need to understand when working with coc.nvim.

## Can't get keywords completion items from other buffer.

The buffer source only provide keywords from buffers meet these conditions:

* Should be loaded by vim, use `:ls` to get buffer list and make sure there's `h` for hidden buffers, you can also use `bufloaded` function to check if a buffer is loaded.
* The `buftype` option should be empty, check it by `:echo &buftype` in your buffer.
* The file associated with buffer should not be git ignored, add `"coc.source.buffer.ignoreGitignore": false` to your config file if you want keywords from git ignored files.

## Environment node doesn't meet the requirement.

Use `let g:coc_node_path = '/path/to/node'` to make coc.nvim use custom node executable.

## Highlight of floating window doesn't looks right.

Some colorscheme use high contrast background colors for `Pmenu` which is linked with `CocFloating` used for floating window highlight, you can overwrite `CocFloating` with a low contrast background color or use colorscheme that works fine by default, for example: https://github.com/morhetz/gruvbox

## Floating window position is wrong after scroll the screen.

It's limitation of (neo)vim, it should either adjust floating window postion according to relative option or provide scroll autocmd so plugin can adjust the window position.

## How could I use omnifunc option to trigger completion of coc.nvim?

You can't, there's no such function provided for omnifunc option, because vim's omnifunc always block and LSP features like triggerCharacters and incomplete response can't work.

If you want to manual trigger completion add `"suggest.autoTrigger": "none",` to coc-settings.json and bind a trigger key like:

``` vim
  inoremap <silent><expr> <c-space> coc#refresh()
```

## How could I disable floating window?

* For documentation of completion, use `"suggest.floatEnable": false` in settings.json.
* For diagnostic messages, use `"diagnostic.messageTarget": "echo"` in settings.json.
* For signature help, use `"signature.target": "echo"` in settings.json.
* For documentation on doHover, use `"coc.preferences.hoverTarget": "echo"` in settings.json.

## Highlight of background seems wrong with floating window.

It's caused by some highlight group using `Normal` for it's background color,
you can overwrite the highlight group to use transparent background color in your vimrc, like:

``` vim
hi Quote ctermbg=109 guifg=#83a598
```
make sure add the lines after `:colorscheme` command.

## How to use coc.nvim from master branch?

* Make sure you don't have `{"tag": "*"}` in `Plug` command if you're using vim-plug as plugin manager.
* Use `Plug 'neoclide/coc.nvim', {'do': 'yarn install --frozen-lockfile'}` in your vimrc if you're using vim-plug.
* Add `let g:coc_force_debug = 1` to your vimrc to make sure coc uses compiled code.
* Update coc.nvim with `:PlugUpdate` command if you're using vim-plug, for other plugin managers run `:call coc#util#install()` after plugin update.
* Run `:echo coc#util#job_command()` to get command used for start coc.nvim service.
* Run `:checkhealth` when you have an issue with neovim.

## How to make preview window shown aside with pum?

For vim, make sure `echo has('textprop') && has('patch-8.1.1522')` echo `1`.

Higlight on vim doesn't always work yet, because can't highlight by using a seperated vim process.

For neovim, build neovim from master code or [use nightly build](https://github.com/neovim/neovim/releases/tag/nightly).

Make sure `:echo exists('##CompleteChanged') && exists('*nvim_open_win')` echo `1`.

To preview expanded snippet body, you can use [coc-snippets](https://github.com/neoclide/coc-snippets).

If you have errors including:

> Wrong number of arguments: expecting 5 but got 3.

It means you're using an old version of neovim. Upgrade to master to avoid this issue.

## My pum flickers when typing.

Use latest neovim/vim which support `equal` field of completion item, checkout `:h complete-items` to see if it's supported.

## My vim is blocked sometimes.

* Make sure your have `set hidden` in your `.vimrc`.
* Use `CocActionAsync` instead of `CocAction` in your autocmd, except for `BufWritePre`.
* Use `CocRequestAsync` instead of `CocRequest` when possible.
* Don't make request to coc before the service initialized. Use `autocmd User CocNvimInit` to send request when you want to send request at startup.

## Neovim crashes when `rightleft` is on.

It's bug of neovim.  Checkout https://github.com/neovim/neovim/issues/9542 for a patch of neovim.

## Language server doesn't work unsaved buffer.

Some language servers don't work when the buffer not saved to disk. This is because they only tested on VSCode which always create file before create buffer.

Save to buffer to disk and restart coc by `:CocRestart` to make the language server work.

## Function parameter/auto import not work on complete done.

* Make sure the language server your're using have support for function snippet/auto import.
* You must use confirm completion to make coc does extra edit after complete done, which is `<C-y>` by default.
* Some language servers are known to have issues with responsed textEdit on completion.
* Some language servers doesn't have support for textEdit on completion but coc extension does, so it's better to use extension when it exists.

## Linting is slow.

If the messages get shown after several seconds, add `set updatetime=300` to your `init.vim` or `.vimrc` and restart vim.

Some language servers are known to have performance issues, including:

https://github.com/palantir/python-language-server

https://github.com/sourcegraph/go-langserver.

Please don't report an issue here when you find those language servers are slow.

By default, coc doesn't show diagnostics in UI when you're in insert mode, 
add `"diagnostic.refreshOnInsertMode": true` in settings file to enable refresh on insert mode.

## Sign of diagnostics not shown.

If there are other signs that have higher offset, sign of coc.nvim can't be shown. You can change the offset of coc.nvim's signs by add `"diagnostic.signOffset": 9999999` to your coc-settings.json to make it higher priority. The default value is 1000. You can also change the `signcolumn` option (available with latest neovim) like: `set signcolumn=auto:2`.

## Not working after upgrade node.

Run command `:CocRebuild` to rebuild coc extensions. Some of them could be using C++ addons, which require a  rebuild after upgrade.

If it's still not working, you can force coc to start with a specified node executable. In your .vimrc add a line like `let g:coc_node_path = '/usr/local/opt/node@10/bin/node'`

## Why vim's location list not work sometimes?

Some plugins like [ale](https://github.com/w0rp/ale) will clear location lists that are created by other plugins. Check out https://github.com/w0rp/ale/issues/1945, it's recommended to use `CocList diagnostics` to get all location lists of diagnostics instead of using location list.

## No completion triggered after type trigger character sometimes.

Some language servers can be slow for receiving document change before trigger completion. You can change the wait time for language server to finish the document change process before completion by changing `coc.preferences.triggerCompletionWait` in your `coc-settings.json`, it's default to `60` in milliseconds.

## My custom key-mapping is not working.

Some plugins like [Ultisnips](https://github.com/SirVer/ultisnips) and [vim-closer](https://github.com/rstacruz/vim-closer) would remap your `<tab>` or `<cr>` without configuration. You can checkout your keymap by command like `:verbose imap <tab>`.

## How could I profile vim.

* Enter these commands:
  ``` vim
  :profile start profile.log
  :profile func *
  :profile file *
  ```
* Make issue happen.
* Exit vim, and open the newly generated profile.log file in your current directory.

## How to change highlight of diagnostic signs?

The sign highlight groups are `CocErrorSign` `CocWarningSign` `CocInfoSign` `CocHintSign`

You can customize the highlight by adding something like:

``` vim
highlight link CocErrorSign GruvboxRed
```
in your `.vimrc`.

See `:h coc-highlights` for all highlight groups.

## How to get log of coc.nvim?

Enable debug mode for coc in your terminal:

```
export NVIM_COC_LOG_LEVEL=debug
```

Open log file by command:
```
:CocOpenLog
```