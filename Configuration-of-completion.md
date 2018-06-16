[Trigger mode of completion](#trigger-mode-of-completion)
[Use `<Tab>` or custom key for trigger completion](#use-tab-or-custom-key-for-trigger-completion)
[Improve completion experience](#improve-completion-experience)

***

COC support different kinds of completion modes, and should works well with your vim completion options.

## Trigger mode of completion

COC have 3 different trigger modes:

* `always`, the default mode, which trigger completion on both first word letter inserted and respect the `triggerCharacters` defined by sources.
* `trigger`, only trigger completion on `triggerCharacters` defined by sources inserted.
* `none`, disable auto trigger completion, you will have to trigger the completion by yourself.

You can change trigger mode by [using configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-configuration-file)

## Use `<Tab>` or custom key for trigger completion

You can make use of `coc#refresh()` for trigger completion like:

``` vim
function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~ '\s'
endfunction

inoremap <silent><expr> <TAB>
      \ pumvisible() ? "\<C-n>" :
      \ <SID>check_back_space() ? "\<TAB>" :
      \ coc#refresh()
```
or
```
imap <c-space> coc#refresh()
```

You can have automatic trigger and custom trigger keys work together.

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
