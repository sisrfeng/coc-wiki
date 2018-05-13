## Why `omni` source is not enabled by default?

This is because `omni` function runs as vim script, it could be really slow and block your UI, if you're working on some complicated language, it's recommended to use async source, for example: the LSP based ones or somehow using a server for async communication. COC make the filetypes of `omni` source as user defined, so you could easily switch to server based sources without overhead. You could define the filetypes of `omni` source like this:

``` vim
  let g:coc_source_config = {
        \  'omni': {
        \    'filetypes': ['css', 'html']
        \  },
        \}
``` 
This could enable COC to run `omni` functions on filetype `css` and `html`.

BTW: The recommended css and html completion plugins are [othree/csscomplete.vim](https://github.com/othree/csscomplete.vim) [othree/html5.vim](https://github.com/othree/html5.vim)

## `omni` is enabled for current filetype, but I can't get any result from `omni` source.

Some `omni` function implementation could be broken and they could affect how COC works, so they have to be blacklisted, the current blacklist is: `LanguageClient#complete`.

Get us feedback if you found any `omnifunc` that not works with COC, thanks!

BTW: This concept is borrowed from [deoplete.nvim](https://github.com/Shougo/deoplete.nvim/blob/5d78e1a75d36a719f1f66ee78c635ea05df72b8c/rplugin/python3/deoplete/source/omni.py#L63)