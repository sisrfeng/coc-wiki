## Highlights of coc.nvim's completion

* **Full LSP completion support**, especially snippet and `additionalTextEdit` feature.
* **Completion resolving on completion item change**, it does asynchronous completion resolve on completion item change and the detail and documentation will be shown in a float window when possible.
* **Asynchronous and parallel completion request**. Unless using vim sources, your vim will never be blocked.
* **Incomplete request and cancel request support**, only incomplete completion requests will be triggered on filtering completion items and cancellation requests are sent to servers only when necessary.
* **Real-time buffer keywords**. Coc will generate buffer keywords on buffer change in the background (with debounce), while some completion engines use a cache which isn't always correct.  Plus, [Locality bonus feature](https://code.visualstudio.com/docs/editor/intellisense#_locality-bonus) from VSCode is enabled by default.
* **Filter completion items when possible.** When you do a fuzzy filter with completion items, some completion engines will trigger a new completion, but coc.nvim will filter the items when possible, which makes it much faster.

## Trigger mode of completion

There are 3 different trigger modes:

* `always`, the default mode, which triggers completion on a letter inserted or `triggerCharacters` (or trigger pattern match) defined by the current activated sources.
* `trigger`, only trigger completion when you type `triggerCharacters` (or trigger pattern match) defined by the completion sources.
* `none`, disable auto trigger completion, you will have to trigger the completion manually.

Lots of completion behavior can be changed by [using the configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-the-configuration-file), check out `:h coc-config-suggest` for details.

## Limitation of coc.nvim's completion

There's no function of coc.nvim that can be used as `omnifunc` because it's not possible to support all LSP completion features when using `omnifunc`.

For features like `textEdit` and `additionalTextEdits`(mostly used by automatic import feature) of LSP to work, you **have to** confirm completion by `coc#pum#confirm()`

## Use `<cr>` to confirm completion

You have to remap `<cr>` to make it confirms completion.

``` vim
inoremap <expr> <cr> coc#pum#visible() ? coc#pum#confirm() : "\<CR>"
```

To make `<cr>` select the first completion item and confirm the completion when no item has been selected:

``` vim
inoremap <silent><expr> <cr> coc#pum#visible() ? coc#_select_confirm() : "\<C-g>u\<CR>"
```

**Note:** `\<C-g>u` is used to break undo level.
   
To make coc.nvim format your code on `<cr>`:

``` vim
inoremap <silent><expr> <cr> coc#pum#visible() ? coc#_select_confirm() : "\<C-g>u\<CR>\<c-r>=coc#on_enter()\<CR>"
```

Use `coc#pum#info()` if you need to confirm completion, only when there's selected complete item:

```vim
inoremap <silent><expr> <cr> coc#pum#visible() && coc#pum#info()['index'] != -1 ? coc#pum#confirm() : "\<C-g>u\<CR>"
```

## Use `<Tab>` or custom key for trigger completion

You can make use of `coc#refresh()` to trigger completion like this:

``` vim
" use <tab> for trigger completion and navigate to the next complete item
function! CheckBackspace() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~# '\s'
endfunction

inoremap <silent><expr> <Tab>
      \ coc#pum#visible() ? coc#pum#next(1) :
      \ CheckBackspace() ? "\<Tab>" :
      \ coc#refresh()
```

**Note:** the `<tab>` could be remapped by another plugin, use `:verbose imap <tab>` to check if it's mapped as expected.

``` vim
" use <c-space>for trigger completion
inoremap <silent><expr> <c-space> coc#refresh()
" Use <C-@> on vim
inoremap <silent><expr> <c-@> coc#refresh()
```

## Use `<Tab>` and `<S-Tab>` to navigate the completion list:

``` vim
inoremap <expr> <Tab> coc#pum#visible() ? coc#pum#next(1) : "\<Tab>"
inoremap <expr> <S-Tab> coc#pum#visible() ? coc#pum#prev(1) : "\<S-Tab>"
```

## Completion sources

Use command `:CocList sources` to get current completion source list.

### Bundled sources.

Name         |Shortcut| Description                                             
------------ |--------| -------------                                           
`around`     |[A]     |Words of the current buffer.                                
`buffer`     |[B]     |Words of other open buffers.                           
`file`       |[F]     |Filename completion, auto-detected.  

### Configuring sources

You can configure completion sources by using coc-settings.json

* `"coc.source.{name}.enable"`: controls if the source is enabled.
* `"coc.source.{name}.shortcut"`: shortcut used in completion menu.
* `"coc.source.{name}.priority"`: priority of the source, lower priority sources will be sorted after high priority sources when they have same score.
* `"coc.source.{name}.disableSyntaxes"`: syntax names used to disable source from completion, eg: `["comment", "string"]`        
* `"coc.source.around.firstMatch"`: Filter complete items by first letter strict match, default `true`.
* `"coc.source.buffer.firstMatch"`: Filter complete items by first letter strict match, default `true`.
* `"coc.source.buffer.ignoreGitignore"`: Ignore git ignored files for buffer words, default `true`.
* `"coc.source.file.trimSameExts"`: Filename extensions to trim extension names on file completion, default `[".ts", ".js"]`
* `"coc.source.file.ignoreHidden"`: Ignore completion for hidden files, default `true`.
* `coc.source.file.ignorePatterns`: Ignore patterns of matcher module, default `[]`.


### More sources

- [coc-sources](https://github.com/neoclide/coc-sources): includes some common
  completion source extensions.
- [coc-neco](https://github.com/neoclide/coc-neco): viml completion support.
- [coc-snippets](https://github.com/neoclide/coc-snippets): snippets solution for coc.nvim
- [coc-vimtex](https://github.com/neoclide/coc-vimtex): vimtex integration.
- [coc-neoinclude](https://github.com/jsfaint/coc-neoinclude): neoinclude
  integration.
- [coc-lbdbq](https://github.com/zidhuss/coc-lbdbq): email address completion.
- [coc-browser](https://github.com/voldikss/coc-browser): web browser words completion.
- [coc-github-users](https://github.com/cb372/coc-github-users): GitHub username completion.
- [coc-db](https://github.com/kristijanhusak/coc-db): Database completion.
- [sphinx.nvim](https://github.com/stsewd/sphinx.nvim): Source for Sphinx's cross-referencing roles.
