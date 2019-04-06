Coc has snippets support in different ways:

* Snippet completion items from different vim snippet plugins, by use extension like: [coc-ultisnips](https://www.npmjs.com/package/coc-ultisnips) and [coc-neosnippet](https://www.npmjs.com/package/coc-neosnippet).
* Snippet kind of completion item from language servers, which are snipmate format.
* Snippet completion items from coc extensions that contribute VSCode snippets.

## Snippet completion

Complete item of snippet kind would be shown with `~` appended in label by default:

![](https://user-images.githubusercontent.com/251450/42562999-b4eb9634-852f-11e8-9f61-bab2bc19db3f.png)

Note: when snippet format of complete item is set on completion resolve, you won't see `~`, since it's impossible for vim to update label of complete item during completion.

The snippet is designed to expand only when the `completionDone` is triggered by using `<C-y> `for confirm, so that user could decide expand the snippet or not. To make `<cr>` for confirm completion, add

``` vim
inoremap <expr> <cr> pumvisible() ? "\<C-y>" : "\<CR>"
```
to your `init.vim`.

A snippet session would cancel under the following conditions:

* `InsertEnter` triggered outside snippet.
* Content change at final placeholder.
* Content added after snippet.
* Content changed in a snippet but not happens to a placeholder.

You can nest snippets in an active snippet session, just like VSCode.

## Configure snippets workflow

To navigate forward/backward of a snippet placeholder, use `<C-j>` and `<C-k>`.
Vim global variable `g:coc_snippet_next` and `g:coc_snippet_prev` can be used to change the key-mapping.

If you don't like `~` as snippet indicator of complete item in completion menu, you can change that by using `coc.preferences.snippetIndicator` in your `coc-settings.json`.

To make snippet completion work just like VSCode, you need to install [coc-snippets](https://github.com/neoclide/coc-snippets) then configure your `<tab>` in vim like:

``` vim
inoremap <silent><expr> <TAB>
      \ pumvisible() ? coc#_select_confirm() :
      \ coc#expandableOrJumpable() ? coc#rpc#request('doKeymap', ['snippets-expand-jump','']) :
      \ <SID>check_back_space() ? "\<TAB>" :
      \ coc#refresh()

function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~# '\s'
endfunction

let g:coc_snippet_next = '<Tab>'
let g:coc_snippet_prev = '<S-Tab>'
```

And add:

``` jsonc
  // make vim select first item on completion
  "suggest.noselect": false
```
to your `coc-settings.json`.

## Using VSCode snippet from extensions

To load VSCode snippets, you need install [coc-snippets](https://github.com/neoclide/coc-snippets) extension.

Then install a VSCode snippet extension from GitHub with a command like:

```
:CocInstall https://github.com/andys8/vscode-jest-snippets.git#master
```

Open a file with a snippet related filetype, like `foo.js` as `javascript` and type part of the prefix characters.

<img width="724" alt="screen shot 2018-09-27 at 2 25 41 pm" src="https://user-images.githubusercontent.com/251450/46127038-edadb280-c261-11e8-8e94-957b6d62c9a9.png">

**Tip** VSCode snippets extension can also installed by use [vim-plug](https://github.com/junegunn/vim-plug)
