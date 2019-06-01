Coc has full featured support for completion with LSP and doesn't bother with vim's `completeopt`.

## Trigger mode of the completion

COC has 3 different trigger modes:

* `always`, the default mode, which triggers completion on a word letter inserted and `triggerCharacters` defined by the current activated sources.
* `trigger`, only trigger completion when you type `triggerCharacters` defined by the completion sources.
* `none`, disable auto trigger completion, you will have to trigger the completion manually.

You can change the trigger mode by [using the configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-configuration-file)

To support completion trigger on inserting enter, add
  
    "suggest.triggerAfterInsertEnter": true

to your `coc-settings.json`.
To change the completion timeout, use:
 
    "suggest.timeout": 500,

To make the automatically select the first completed item, use: 

	"suggest.noselect": false,

To make the completion trigger with two input characters, use: 

	"suggest.minTriggerInputLength": 2

To enable the commit characters feature, use: 

	"suggest.acceptSuggestionOnCommitCharacter": true

To change the indicator of snippet items, use:

	"suggest.snippetIndicator": "►"

## Use `<Tab>` or custom key for trigger completion

You can make use of `coc#refresh()` for trigger completion like this:

``` vim
" use <tab> for trigger completion and navigate to the next complete item
function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~ '\s'
endfunction

inoremap <silent><expr> <TAB>
      \ pumvisible() ? "\<C-n>" :
      \ <SID>check_back_space() ? "\<TAB>" :
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
  **Note:** `\<C-g>u` is used to break undo level.
   
  To make `<cr>` select the first completion item and confirm the completion when no item has been selected:
    ``` vim
    inoremap <silent><expr> <cr> pumvisible() ? coc#_select_confirm() : "\<C-g>u\<CR>"
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

* [coc-sources](https://github.com/neoclide/coc-sources)
* [coc-neco](https://github.com/neoclide/coc-neco)
* [coc-snippets](https://github.com/neoclide/coc-snippets)
* [coc-neoinclude](https://github.com/jsfaint/coc-neoinclude)
