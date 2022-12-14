Here are some common problems that you may need to understand when working with coc.nvim.

## Unexpected diagnostics when using easymotion.

Use autocmd like:
```vim
autocmd User EasyMotionPromptBegin :let b:coc_diagnostic_disable = 1
autocmd User EasyMotionPromptEnd :let b:coc_diagnostic_disable = 0
```

## Some highlight groups not work after colorscheme command?

Most color scheme clears existing highlights when loaded, make
sure set your highlights with `ColorScheme` autocmd, like:
```vim
autocmd ColorScheme * call Highlight()

function! Highlight() abort
  hi Conceal ctermfg=239 guifg=#504945
  hi CocSearch ctermfg=12 guifg=#18A3FF
endfunction
```
And you have to use nested autocmd to make `ColorScheme` autocmd
fired when using autocmd to change color scheme.
```vim
autocmd vimenter * ++nested colorscheme gruvbox
```

## The selection highlight not looks good.

`CocMenuSel` is used for highlight select item, the highlight group uses background color from `PmenuSel`. 
However, many color schemes not considered other highlights inside it. 

You can change the highlight group by:
```vim
hi CocMenuSel ctermbg=237 guibg=#13354A
```
in your vimrc, to survive after color scheme change, use:
```vim
autocmd ColorScheme * hi CocMenuSel ctermbg=237 guibg=#13354A
```

## Can't get keywords completion items from other buffer.

The buffer source only provide keywords from buffers meet these conditions:

* Use `bufloaded()` function to check if a buffer is loaded.
* The `buftype` option should be empty, check it by `:echo getbufvar(bufnr, "&buftype")`.

## Environment node doesn't meet the requirement.

Use `let g:coc_node_path = '/path/to/node'` to make coc.nvim use custom node executable.

## How could I use omnifunc option to trigger completion of coc.nvim?

You can't, there's no such function provided for `omnifunc` option, because vim's omnifunc always block and LSP features like triggerCharacters and incomplete response can't work.

If you want to manually trigger completion add `"suggest.autoTrigger": "none",` to coc-settings.json and bind a trigger key like:

``` vim
  inoremap <silent><expr> <c-space> coc#refresh()
```
Note that some terminals send `<NUL>` when you press `<c-space>`, so you could use instead:

``` vim
  inoremap <silent><expr> <NUL> coc#refresh()
```

## My vim is blocked sometimes.

* Make sure you have `set hidden` in your `.vimrc`.
* Use `CocActionAsync` instead of `CocAction` in your autocmd, except for `BufWritePre`.
* Use `CocRequestAsync` instead of `CocRequest` when possible.

## Language server doesn't work with unsaved buffer.

Some language servers don't work when the buffer not saved to disk. This is because they only tested on VSCode which always create file before create buffer.

Save the buffer to disk and restart coc.nvim server by `:CocRestart` to make the language server work.

## Linting is slow.

By default, coc doesn't show diagnostics in UI when you're in insert mode, 
add `"diagnostic.refreshOnInsertMode": true` in settings file to enable refresh on insert mode.

## Sign of diagnostics not shown.

If there are other signs that have higher offset, sign of coc.nvim can't be shown. You can change the offset of coc.nvim's signs by add `"diagnostic.signOffset": 9999999` to your coc-settings.json to make it higher priority. The default value is 1000. You can also change the `signcolumn` option (available with latest neovim) like: `set signcolumn=auto:2`.

## No completion triggered after type trigger character sometimes.

Some language servers can be slow for receiving document change before trigger completion. You can change the wait time for language server to finish the document change process before completion by changing `suggest.triggerCompletionWait` in your `coc-settings.json`, it's default to `50` in milliseconds.

## My key-mapping is not working.

Some plugins like [Ultisnips](https://github.com/SirVer/ultisnips) and [vim-closer](https://github.com/rstacruz/vim-closer) would remap your `<tab>` or `<cr>` without configuration. You can check out your key-mapping by command like `:verbose imap <tab>`.

## How could I profile vim.

* Enter these commands:
  ``` vim
  :profile start profile.log
  :profile func *
  :profile file *
  ```
* Make issue happen.
* Run command `:profile stop` (neovim only).
* Exit vim, and open the newly generated profile.log file in your current directory.

## How could I profile coc.nvim.

To get the communication between vim and coc.nvim

* Add `let g:node_client_debug = 1` in your vimrc.
* Restart vim and make issue happen.
* Use command `:call coc#client#open_log()` to open log file, or use `:echo $NODE_CLIENT_LOG_FILE` to get file path of log.

## How to get log of coc.nvim?

Enable debug mode for coc in your terminal:

```
export NVIM_COC_LOG_LEVEL=debug
```

Open log file by command:
```
:CocOpenLog
```

## How to show documentation of symbol under cursor, also known as cursor hover?

This is done by the `doHover` action. Configure a mapping like, e.g.:

```vim
nnoremap <silent> <leader>h :call CocActionAsync('doHover')<cr>
```

## Cursor disappeared after exit CocList

Possible bug with `guicursor` option on your terminal, you can disable transparent cursor by 
```vim
let g:coc_disable_transparent_cursor = 1
```
in your .vimrc.

## Floating window position is wrong after scroll the screen.

It's expected since the float windows/popups are absolutely positioned.

## How could I disable floating window?

* For documentation of completion, use `"suggest.floatEnable": false` in settings.json.
* For diagnostic messages, use `"diagnostic.messageTarget": "echo"` in settings.json.
* For signature help, use `"signature.target": "echo"` in settings.json.
* For documentation on hover, use `"hover.target": "echo"` in settings.json.

## Highlight of background seems wrong with floating window.

The default highlight group linked by `CocFloating` could have `reverse` attribute, you will have colored background for some texts on that case, define your own `CocFloating` highlight group on that case.

``` vim
hi link CocFloating Normal
```

## How to scroll float window?

Checkout `:h coc#float#has_scroll()`

## How to open link in float window?

Focus the window by `<C-w>w` if it's focusable on neovim and invoke `:call CocAction('openlink')`