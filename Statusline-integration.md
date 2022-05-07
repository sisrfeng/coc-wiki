## Use built-in function

### Coc status (Typescript version and reported issues)
Add section `%{coc#status()}` to your `statusline` option.

Function `coc#status()` includes status information of diagnostic of current buffer and all status messages received from extensions.

### Current Function Symbol
If you want to display the name of the current enclosing symbol (eg. function name), you can use the `b:coc_current_function` buffer-bound variable.

To update its value automatically (updated on `CursorHold`), you need to set `"coc.preferences.currentFunctionSymbolAutoUpdate": true` in your config. 

Alternatively, if you want to handle the updates manually, you can call `CocAction("getCurrentFunctionSymbol")`, which will return the symbol name and update the `b:coc_current_function` variable.

## Use manual function

You can use `b:coc_diagnostic_info` and `g:coc_status` to build your own status function, like:

``` vim
function! StatusDiagnostic() abort
  let info = get(b:, 'coc_diagnostic_info', {})
  if empty(info) | return '' | endif
  let msgs = []
  if get(info, 'error', 0)
    call add(msgs, 'E' . info['error'])
  endif
  if get(info, 'warning', 0)
    call add(msgs, 'W' . info['warning'])
  endif
  return join(msgs, ' '). ' ' . get(g:, 'coc_status', '')
endfunction
```
Then add `%{StatusDiagnostic()}` ` to your 'statusline' option.

## Integration with [eleline.vim](https://github.com/liuchengxu/eleline.vim)

It just works.

## Integration with [vim-airline](https://github.com/vim-airline/vim-airline)

airline now works out of box.

Check out `:h airline-coc` for customization details.

## Integration with [lightline.vim](https://github.com/itchyny/lightline.vim)

Use configuration like:

``` vim
function! CocCurrentFunction()
    return get(b:, 'coc_current_function', '')
endfunction

let g:lightline = {
      \ 'colorscheme': 'wombat',
      \ 'active': {
      \   'left': [ [ 'mode', 'paste' ],
      \             [ 'cocstatus', 'currentfunction', 'readonly', 'filename', 'modified' ] ]
      \ },
      \ 'component_function': {
      \   'cocstatus': 'coc#status',
      \   'currentfunction': 'CocCurrentFunction'
      \ },
      \ }
```