Coc has snippets support in different ways:

* Snippet completion items from different vim snippet plugins, by using extension like: [coc-ultisnips](https://www.npmjs.com/package/coc-ultisnips) and [coc-neosnippet](https://www.npmjs.com/package/coc-neosnippet).
* Snippet kind of completion item from language servers, which are snipmate format.
* Snippets from coc.nvim extensions that contribute VSCode snippets (requires [coc-snippets](https://github.com/neoclide/coc-snippets)).

## Snippet completion

Complete item of snippet kind would be shown with `~` appended in label by default:

![](https://user-images.githubusercontent.com/251450/42562999-b4eb9634-852f-11e8-9f61-bab2bc19db3f.png)

**Note:** when snippet format of complete item is set on completion resolve, you won't see `~`, it's limitation of vim.

The snippet is designed to expand only when the `completionDone` is triggered by using `<C-y> `for confirm, so that user could decide expand the snippet or not. 

To make `<cr>` for confirm completion, add

``` vim
inoremap <silent><expr> <cr> pumvisible() ? coc#_select_confirm() : 
                                           \"\<C-g>u\<CR>\<c-r>=coc#on_enter()\<CR>"
```
to your vimrc.

* `coc#_select_confirm()` helps select first completion item when necessary and send `<C-y>` to vim for confirm completion.
* `\<C-g>u` used for break undo chain at current position.
* `coc#on_enter()` notify coc that you have pressed `<enter>`, so it can format your code on `<enter>`.

**Note:** some plugins are known to overwrite your `<CR>` mappings, so your custom keymap won't work, checkout your `<CR>` mapping by `:verbose imap <CR>`. 

A snippet session would cancel under the following conditions:

* `InsertEnter` triggered outside snippet.
* Content change at final placeholder.
* Content added after snippet.
* Content changed in a snippet but not happens to a placeholder.

You can nest snippets in an active snippet session, just like VSCode.

If you don't want to expand snippet on completion, just use `<C-n>` to insert the extracted text without confirm completion, or checkout options of your language server to disable snippet complete items.

## Configure snippets workflow

To navigate forward/backward of a snippet placeholder, use `<C-j>` and `<C-k>`.
Vim global variable `g:coc_snippet_next` and `g:coc_snippet_prev` can be used to change the key-mapping.

If you don't like `~` as snippet indicator of complete item in completion menu, you can change that by using `suggest.snippetIndicator` in your `coc-settings.json`.

To make snippet completion work just like VSCode, you need to install [coc-snippets](https://github.com/neoclide/coc-snippets) then configure your `<tab>` in vim like:

``` vim
inoremap <silent><expr> <TAB>
      \ pumvisible() ? coc#_select_confirm() :
      \ coc#expandableOrJumpable() ? "\<C-r>=coc#rpc#request('doKeymap', ['snippets-expand-jump',''])\<CR>" :
      \ <SID>check_back_space() ? "\<TAB>" :
      \ coc#refresh()

function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~# '\s'
endfunction

let g:coc_snippet_next = '<tab>'
```

And add:

``` jsonc
  // make vim select first item on completion
  "suggest.noselect": false
```
to your `coc-settings.json`.

Others configurations:

- `coc.preferences.snippetStatusText`: Text shown in 'statusline' to indicate snippet session is activate.
- `coc.preferences.snippetHighlight`: When true, use highlight group 'CocSnippetVisual' to highlight placeholders with same index of current one.

## Using VSCode snippet from extensions

To load VSCode snippets, you need install [coc-snippets](https://github.com/neoclide/coc-snippets) extension.

Then install a VSCode snippet extension from GitHub with a command like:

```
:CocInstall https://github.com/andys8/vscode-jest-snippets
```

Open a file with a snippet related filetype, like `foo.js` as `javascript` and type part of the prefix characters.

<img width="724" alt="screen shot 2018-09-27 at 2 25 41 pm" src="https://user-images.githubusercontent.com/251450/46127038-edadb280-c261-11e8-8e94-957b6d62c9a9.png">

**Tip** VSCode snippets extension can also be installed by using [vim-plug](https://github.com/junegunn/vim-plug)
