Coc have snippets support in different ways:

* Snippet completion items from different vim snippet plugins, including [ultisnips](https://github.com/SirVer/ultisnips) and [neosnippet.vim](https://github.com/Shougo/neosnippet.vim).
* Snippet kind of completion item from language servers.
* Snippet completion items from coc extensions that contribute VSCode snippets.

## Snippet completion

Complete item of snippet kind would be shown with ~ appended:
![](https://user-images.githubusercontent.com/251450/42562999-b4eb9634-852f-11e8-9f61-bab2bc19db3f.png)

The snippet is designed to expand only when the `completionDone` is triggered by using <C-y> for confirm, so that user could decide expand the snippet or not. To make `<cr>` for confirm completion, add
``` vim
inoremap <expr> <cr> pumvisible() ? "\<C-y>" : "\<C-g>u\<CR>"
```
to your `init.vim`.

## Configuring snippet completion

To navigate forward/backward of snippet placeholder, use `<C-j>` and `<C-k>`.
Vim global variable `g:coc_snippet_next` and `g:coc_snippet_prev` can be used to change the keymapping.

For configure complete source of `ultisnip` and `neosnippet` try type `ultisnips` and `neosnippet` in your `coc-settings.json` to get completion for all related options.

## Using VSCode snippet extension

Coc could use snippets from VSCode extensions.

First, install a VSCode snippet extension from github by command like:

```
:CocInstall https://github.com/andys8/vscode-jest-snippets.git#master
```

Open file with snippet related filetype, like `foo.js` as `javascript`, try complete by type part of prefix characters.

<img width="724" alt="screen shot 2018-09-27 at 2 25 41 pm" src="https://user-images.githubusercontent.com/251450/46127038-edadb280-c261-11e8-8e94-957b6d62c9a9.png">
