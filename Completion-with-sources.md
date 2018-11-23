Coc have full featured support for completion of LSP, and doesn't bother `completeopt` of vim.

# Contents

* [Trigger mode of completion](#trigger-mode-of-completion)
* [Use `<Tab>` or custom key for trigger completion](#use-tab-or-custom-key-for-trigger-completion)
* [Improve completion experience](#improve-completion-experience)
* [Completion sources](#completion-sources)

## Trigger mode of completion

COC have 3 different trigger modes:

* `always`, the default mode, which trigger completion on word letter inserted, insert enter and `triggerCharacters` defined by sources inserted.
* `trigger`, only trigger completion on `triggerCharacters` defined by completion sources.
* `none`, disable auto trigger completion, you will have to trigger the completion by yourself.

You can change trigger mode by [using configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-configuration-file)

To support completion trigger on insert enter, add
  
    "coc.preferences.triggerAfterInsertEnter": true

to your `coc-settings.json`.

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

### Bundled sources.

Name         | Description                                             
------------ | -------------                                           
`around`     | Words of current buffer.                                
`buffer`     | Words of none current buffer.                           
`file`       | Filename completion, auto detected.                    

### More sources

* [coc-sources](https://github.com/neoclide/coc-sources)
* [coc-neco](https://github.com/neoclide/coc-neco)
