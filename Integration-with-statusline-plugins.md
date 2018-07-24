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

```
