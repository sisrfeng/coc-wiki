Coc.nvim has full support for LSP completion, including snippet and additional text edits support.

By default, coc.nvim use its own `completeopt` option during completion to provide the best auto completion experience.

There's no function can be used as `omnifunc` option provided from coc.nvim since it's not possible to support all LSP completion features when using `omnifunc` option.

## Trigger mode of the completion

There're 3 different trigger modes:

* `always`, the default mode, which triggers completion on a word letter inserted and `triggerCharacters` (or trigger pattern match) defined by the current activated sources.
* `trigger`, only trigger completion when you type `triggerCharacters` (or trigger pattern match) defined by the completion sources.
* `none`, disable auto trigger completion, you will have to trigger the completion manually.

Some completion behavior can be changed by [using the configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-the-configuration-file).

To change the trigger mode, add:

    "suggest.autoTrigger": "trigger"

To support completion trigger on insert enter, add
  
    "suggest.triggerAfterInsertEnter": true

To change the completion timeout, use:
 
    "suggest.timeout": 500,

To make the completion automatically select the first completed item, use: 

    "suggest.noselect": false,

To make the completion trigger with two input characters, use: 

    "suggest.minTriggerInputLength": 2

To enable the commit characters feature, use: 

    "suggest.acceptSuggestionOnCommitCharacter": true

To change the indicator of snippet items, use:

    "suggest.snippetIndicator": "►"

To get the full list checkout the help by `:h coc-configuration` in your vim.

## Use `<Tab>` or custom key for trigger completion

You can make use of `coc#refresh()` for trigger completion like this:

``` vim
" use <tab> for trigger completion and navigate to the next complete item
function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~ '\s'
endfunction

inoremap <silent><expr> <Tab>
      \ pumvisible() ? "\<C-n>" :
      \ <SID>check_back_space() ? "\<Tab>" :
      \ coc#refresh()
```

**Note:** the `<tab>` could be remapped by another plugin, use `:verbose imap <tab>` to check if it's mapped as expected.

``` vim
" use <c-space>for trigger completion
inoremap <silent><expr> <c-space> coc#refresh()
```

## Improve the completion experience

* Use `<Tab>` and `<S-Tab>` to navigate the completion list:

   ``` vim
   inoremap <expr> <Tab> pumvisible() ? "\<C-n>" : "\<Tab>"
   inoremap <expr> <S-Tab> pumvisible() ? "\<C-p>" : "\<S-Tab>"
   ```

* Use `<cr>` to confirm completion
    ``` vim
    inoremap <expr> <cr> pumvisible() ? "\<C-y>" : "\<C-g>u\<CR>"
    ```
  **Note:** you have to remap `<cr>` to make sure it confirm completion when pum is visible.

  **Note:** `\<C-g>u` is used to break undo level.
   
  To make `<cr>` select the first completion item and confirm the completion when no item has been selected:
    ``` vim
    inoremap <silent><expr> <cr> pumvisible() ? coc#_select_confirm() : "\<C-g>u\<CR>"
    ```
  To make coc.nvim format your code on `<cr>`, use keymap:

    ``` vim
    inoremap <silent><expr> <cr> pumvisible() ? coc#_select_confirm() : "\<C-g>u\<CR>\<c-r>=coc#on_enter()\<CR>"
    ```

* Close the preview window when completion is done.
    ``` vim
    autocmd! CompleteDone * if pumvisible() == 0 | pclose | endif
    ```
## Completion sources

### Bundled sources.

Name         |Shortcut| Description                                             
------------ |--------| -------------                                           
`around`     |[A]     |Words of the current buffer.                                
`buffer`     |[B]     |Words of none current buffer.                           
`file`       |[F]     |Filename completion, auto detected.  

### Configuring sources

You can configure completion sources by using coc-settings.json

* `"coc.source.{name}.enable"`: controls if the source is enabled.
* `"coc.source.{name}.shortcut"`: shortcut used in completion menu.
* `"coc.source.{name}.priority"`: priority of the source, lower priority sources will be sorted after high priority sources when they have same score.
* `"coc.source.{name}.disableSyntaxes"`: syntax names used to disable source from completion, eg: `["comment", "string"]`        

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