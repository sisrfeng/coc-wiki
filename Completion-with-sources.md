COC support different kinds of completion modes, and should works well with your `completeopt` of vim.

# Contents

* [Trigger mode of completion](#trigger-mode-of-completion)
* [Snippet completion](#snippet-completion)
* [Use `<Tab>` or custom key for trigger completion](#use-tab-or-custom-key-for-trigger-completion)
* [Improve completion experience](#improve-completion-experience)
* [Completion sources](#completion-sources)

## Trigger mode of completion

COC have 3 different trigger modes:

* `always`, the default mode, which trigger completion on both first word letter inserted and respect the `triggerCharacters` defined by sources.
* `trigger`, only trigger completion on `triggerCharacters` defined by completion sources.
* `none`, disable auto trigger completion, you will have to trigger the completion by yourself.

You can change trigger mode by [using configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-configuration-file)

## Snippet completion

Coc support snippet out of box, the snippet item would be shown with `~` appended:

<img width="298" alt="screen shot 2018-07-11 at 5 27 09 pm" src="https://user-images.githubusercontent.com/251450/42562999-b4eb9634-852f-11e8-9f61-bab2bc19db3f.png">

The snippet is designed to expanded only when the completionDone is triggered by using `<C-y>` for confirm, so that user could decide expand the snippet or not.

To navigate forward/backward of snippet placeholder, user could use `<C-j>` and `<C-k>` (could be changed by setting `g:coc_snippet_next` and `g:coc_snippet_prev`)

## Use `<Tab>` or custom key for trigger completion

You can make use of `coc#refresh()` for trigger completion like:

``` vim
" use <tab> for trigger completion and navigate next complete item
function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~ '\s'
endfunction

inoremap <silent><expr> <TAB>
      \ pumvisible() ? "\<C-n>" :
      \ <SID>check_back_space() ? "\<TAB>" :
      \ coc#refresh()
```

``` vim
" use <c-space>for trigger completion
imap <c-space> coc#refresh()
```

## Improve completion experience

* Use `<Tab>` and `<S-Tab>` for navigate completion list:

   ``` vim
   inoremap <expr> <Tab> pumvisible() ? "\<C-n>" : "\<Tab>"
   inoremap <expr> <S-Tab> pumvisible() ? "\<C-p>" : "\<S-Tab>"
   ```

* Use `<enter>` to confirm complete
   ``` vim
   inoremap <expr> <cr> pumvisible() ? "\<C-y>" : "\<cr>"
   ```

* Close preview window when completion is done.
    ``` vim
    autocmd! CompleteDone * if pumvisible() == 0 | pclose | endif
    ```
## Completion sources

There're three types of completion sources,`common`, `vim` and `service`.

* `common` sources are bundled with coc, and implemented in javascript.
* `vim` sources are implemented in viml and could comes from other vim plugins, coc has some bundled vim sources.
* `service` sources are registered by language servers, they have higher priority.

### Bundled common sources.

Name         | Description                                             | Use cache   | Default filetypes
------------ | -------------                                           | ------------|------------
`around`     | Words of current buffer.                                | ✗           | all
`buffer`     | Words of none current buffer.                           | ✓           | all
`dictionary` | Words from files of local `dictionary` option.          | ✓           | all
`tag`        | Words from `taglist` of current buffer.                 | ✓           | all
`file`       | Filename completion, auto detected.                     | ✗           | all
`omni`       | Invoke `omnifunc` of current buffer for complete items. | ✗           | []
`word`       | Words from google 10000 english repo.                   | ✓           | all
`emoji`      | Eomji characters.                                       | ✓           | ['markdown']
`include`    | Full path completion for include file paths.            | ✗           | ['javascript', 'typescript']

`omni` source could be really slow, it requires configuration for `filetypes` to work.

### Bundled vim sources

Vim sources are implemented in viml, and usually requires other vim plugin to work.

Name           |Description                |Filetype     | Requirement
------------   |------------               |------------ | -------------
ultisnips      |Snippets name completion   |User defined | Install [ultisnips](https://github.com/SirVer/ultisnips)
neco           |VimL completion            |vim          | Install [neco-vim](https://github.com/Shougo/neco-vim)

### Bundled service sources

Like VSCode, coc comes with some language server extensions built in, some of them could provide completion for certain filetype(s).

Name         | File types              
------------ | -------------           
tsserver     | typescript, javascript  
html         | html, handlebars, razor 
css          | css, less, scss, wxss             
json         | json, jsonc             
solargraph   | ruby                   
wxml         | wxml                    

