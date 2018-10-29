## Use build in function

Add section `%{coc#status()}` to your `statusline` option.

Function `coc#status()` including status information of diagnostic of current buffer and all status message received from extensions.

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

## Integration [vim-airline](https://github.com/vim-airline/vim-airline)

To make airline show the diagnostic information from coc, you can configure airline like:

``` vim
" if you want to disable auto detect, comment out those two lines
"let g:airline#extensions#disable_rtp_load = 1
"let g:airline_extensions = ['branch', 'hunks', 'coc']

let g:airline_section_error = '%{airline#util#wrap(airline#extensions#coc#get_error(),0)}'
let g:airline_section_warning = '%{airline#util#wrap(airline#extensions#coc#get_warning(),0)}'
```
Check out `:h coc-status-airline` for detail.

## Integration with [lightline.vim](https://github.com/itchyny/lightline.vim)

Use configuration like:

``` vim
let g:lightline = {
      \ 'colorscheme': 'wombat',
      \ 'active': {
      \   'left': [ [ 'mode', 'paste' ],
      \             [ 'cocstatus', 'readonly', 'filename', 'modified' ] ]
      \ },
      \ 'component_function': {
      \   'cocstatus': 'coc#status'
      \ },
      \ }
```


