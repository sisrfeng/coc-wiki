Coc have full featured support for completion of LSP, and doesn't bother `completeopt` of vim.

## Trigger mode of completion

COC have 3 different trigger modes:

* `always`, the default mode, which trigger completion on word letter inserted and `triggerCharacters` defined by current activated sources.
* `trigger`, only trigger completion when type `triggerCharacters` defined by completion sources.
* `none`, disable auto trigger completion, you will have to trigger the completion manually.

You can change trigger mode by [using configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-configuration-file)

To support completion trigger on insert enter, add
  
    "suggest.triggerAfterInsertEnter": true

to your `coc-settings.json`.
To change the timeout of completion, use:
 
    "suggest.timeout": 500,

To make the first complete item selected automatically, use: 

	"suggests.noselect": false,

To make completion triggered with two input characters, use: 

	"suggest.minTriggerInputLength": 2

To enable commit characters feature, use: 

	"suggest.acceptSuggestionOnCommitCharacter": true

To change the indicator of snippet item, use:

	"suggest.snippetIndicator": "►"

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

**Note:** the `<tab>` could be mapped by other plugin, use `:verbose imap <tab>` to check it's mapped as expected.

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

* Use `<cr>` to confirm complete
    ``` vim
    inoremap <expr> <cr> pumvisible() ? "\<C-y>" : "\<C-g>u\<CR>"
    ```
  **Note:** `\<C-g>u` is used to break undo level.
   
  To make `<cr>` select the first completion item and confirm completion when no item have selected:
    ``` vim
    inoremap <silent><expr> <cr> pumvisible() ? coc#_select_confirm() : "\<C-g>u\<CR>"
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
* [coc-snippets](https://github.com/neoclide/coc-snippets)
* [coc-neoinclude](https://github.com/jsfaint/coc-neoinclude)
