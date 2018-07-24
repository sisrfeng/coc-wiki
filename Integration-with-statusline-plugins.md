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

## Integration with [vim-airline](https://github.com/vim-airline/vim-airline)

Use configuration like:

``` vim
  " if you want to disable, comment out those two lines
  "let g:airline#extensions#disable_rtp_load = 1
  "let g:airline_extensions = ['branch', 'hunks', 'coc']

  let g:airline_section_error = '%{airline#util#wrap(airline#extensions#coc#get_error(),0)}'
  let g:airline_section_warning = '%{airline#util#wrap(airline#extensions#coc#get_warning(),0)}'
```
