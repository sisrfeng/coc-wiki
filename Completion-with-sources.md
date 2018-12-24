Coc have full featured support for completion of LSP, and doesn't bother `completeopt` of vim.

## Highlights of coc completion

* All completion sources runs in parallel.
* Use buffer update events of neovim for synchronize buffer contents.
* Trigger async completion without timer, does filter with new input on completion finished . 
* Filter previous completion items when possible to prevent unnecessary completion request.
* Filter completion items on `<backspace>` when possible.
* Full support of completion item definition in LSP, including `snippetTextFormat`, `additionalTextEdit`, `sortText` etc.
* Always fuzzy match and smart case (lower case letter is case insensitive, upper case letter must have strict match).
* Does filter on TextChangedP when necessary.
* Does completion resolve on change completion item, echo detail when found.
* Change and restore your `completeopt` option during completion (you can keep use `preview,menu` for other completion).

## Trigger mode of completion

COC have 3 different trigger modes:

* `always`, the default mode, which trigger completion on word letter inserted and `triggerCharacters` defined by current activated sources.
* `trigger`, only trigger completion when type `triggerCharacters` defined by completion sources.
* `none`, disable auto trigger completion, you will have to trigger the completion manually.

You can change trigger mode by [using configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-configuration-file)

To support completion trigger on insert enter, add
  
    "coc.preferences.triggerAfterInsertEnter": true

to your `coc-settings.json`.
To change the timeout of completion, use:

	"coc.preferences.timeout": 500,

To make the first complete item selected automatically, use: 

	"coc.preferences.noselect": false,

To make completion triggered with two input characters, use: 

	"coc.preferences.minTriggerInputLength": 2

To enable commit characters feature, use: 

	"coc.preferences.acceptSuggestionOnCommitCharacter": true

To change the indicator of snippet item, use:

	"coc.preferences.snippetIndicator": "⭐︎"

## Use `<Tab>` or custom key for trigger completion

You can make use of `coc#refresh()` for trigger completion like:

``` vim
" use <tab> for trigger completion and navigate to next complete item
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
  To force coc select complete item before confirm, use:
   ``` vim
   inoremap <expr> <cr> pumvisible() ? coc#_select_confirm() : "\<C-g>u\<CR>"
   ```

* Close preview window when completion is done.
    ``` vim
    autocmd! CompleteDone * if pumvisible() == 0 | pclose | endif
    ```
## Completion sources

### Bundled sources.

Name         | Description                                             
------------ | -------------                                           
`around`     | Words of current buffer.                                
`buffer`     | Words of none current buffer.                           
`file`       | Filename completion, auto detected.                    

### More sources

* [coc-sources](https://github.com/neoclide/coc-sources)
* [coc-neco](https://github.com/neoclide/coc-neco)
* [coc-neoinclude](https://github.com/jsfaint/coc-neoinclude)
