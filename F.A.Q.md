Here's some common problems that you may need to understand when working with COC.

## Word of current buffer not shown by around source.

This could happen when the file is larger than 3k lines code, for performance reason, the around source would then use words from the latest saved buffer cache, so it could be not accurate.

## Why `omni` source requires user configuration to work?

This is because `omni` function runs as vim script, it could be really slow and block your UI, if you're working on some complicated language, it's recommended to use async source, for example: the LSP based ones or somehow using a server for async communication. COC make the filetypes of `omni` source as user defined, so you could easily switch to server based sources without overhead. You can define the filetypes of `omni` source like this:

``` vim
  let g:coc_source_config = {
        \  'omni': {
        \    'filetypes': ['css', 'html']
        \  },
        \}
``` 
This could enable COC to run `omni` functions on filetype `css` and `html`.

BTW: The recommended css and html completion plugins are [othree/csscomplete.vim](https://github.com/othree/csscomplete.vim) [othree/html5.vim](https://github.com/othree/html5.vim)

## Why `ultisnips` source requires user configuration to work?

Because completion framework can't tell whether you're completing for snippest or words, it could just slow you down when the popup menu contains all of them. You could simply define a custom complete keymap for UltiSnips like this:

``` vim
" improved ultisnips complete {{
inoremap <C-l> <C-R>=SnipComplete()<CR>
func! SnipComplete()
  let line = getline('.')
  let start = col('.') - 1
  while start > 0 && line[start - 1] =~# '\k'
    let start -= 1
  endwhile
  let suggestions = []
  let snips =  UltiSnips#SnippetsInCurrentScope(0)
  for item in keys(snips)
    let entry = {'word': item, 'menu': snips[item]}
    call add(suggestions, entry)
  endfor
  if empty(suggestions)
    echohl Error | echon 'no match' | echohl None
  elseif len(suggestions) == 1
    let pos = getcurpos()
    if start == 0
      let str = trigger
    else
      let str = line[0:start - 1] . trigger
    endif
    call setline('.', str)
    let pos[2] = len(str) + 1
    call setpos('.', pos)
    call UltiSnips#ExpandSnippet()
  else
    call complete(start + 1, suggestions)
  endif
  return ''
endfunc
" }}
```
So then you can use `<C-l>` for snippets completion, it's much faster.  (Maybe I should send a PR to UltiSnips)

However, you can still have ultisnips source enabled for specified filetype by:
``` vim
  let g:coc_source_config = {
        \  'ultisnips': {
        \    'filetypes': ['javascript']
        \  },
        \}
```
Or for all filetypes:
``` vim
  let g:coc_source_config = {
        \  'ultisnips': {
        \    'filetypes': v:null
        \  },
        \}
```
In general, COC is designed for speed & flexible, **not** for the beginners. 

## Why `omni` doesn't work even if enabled in configuration?

First, checkout you have set `omnifunc` option correctly for current filetype by:

``` vim
:verbose set omnifunc
```
COC would ignore `omni` complete silently if the value is not a existing function.

Some `omni` functions could be broken, so they have to be blacklisted, the current blacklist is: `LanguageClient#complete`.

Get us feedback if you found any `omnifunc` that not works with COC, thanks!

BTW: This concept is borrowed from [deoplete.nvim](https://github.com/Shougo/deoplete.nvim/blob/5d78e1a75d36a719f1f66ee78c635ea05df72b8c/rplugin/python3/deoplete/source/omni.py#L63)

## Input highlight not updated when selected complete item changed.

The problem could be resolved after [neovim/pull/8377](https://github.com/neovim/neovim/pull/8377) merged, and you have `TextChangedP` event available in your neovim.

## Is it possible to highlight the characters in complete items?

No, since vim/neovim doesn't have that kind of API, unless you're using [external UIs of neovim](https://github.com/neovim/neovim/wiki/Related-projects#gui) some of them could render complete items themself and provide custom highlight for complete items. 