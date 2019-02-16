Here's some common problems that you may need to understand when working with coc.nvim.

## My pum flick when typing.

Try latest neovim release, coc fix this issue by trigger completion on `InsertCharPre`, but it doesn't work on vim.

## My vim is blocked sometimes.

* Make sure your have `set hidden` in your `.vimrc`.
* Use `CocActionAsync` instead of `CocAction` in your autocmd, except for `BufWritePre`.
* Use `CocRequestAsync` instead of `CocRequest` when possible.
* Don't make request to coc before the service initialized, use `autocmd User CocNvimInit` to send request when you want to send request at startup.

## Neovim crash when `rightleft` is on.

It's bug of neovim.  Checkout https://github.com/neovim/neovim/issues/9542 for a patch of neovim.

## Language server doesn't work unsaved buffer.

Some language server doesn't work when the buffer not saved to disk, this is because they only tested on VSCode which always create file before create buffer.

Save to buffer to disk and restart coc by `:CocRestart` to make the language server work.

## Completion for function parameter not working.

Completion for function parameter requries server send the completion as snippet, some language server doesn't support that. 

## Linting is slow.

Some language server is known to have performance issue sometimes, including https://github.com/palantir/python-language-server, https://github.com/sourcegraph/go-langserver.

Don't report issue here when you found those language server is slow.

For vim, coc doesn't show diagnostics in UI when you're in insert mode to avoid unnecessary redraw.

## Not working after upgrade node.

Run command `:CocRebuild` to rebuild coc extensions, some of them could be using C++ addons, which requires rebuild after upgrade.

If still not working, you can force coc started with specified node executable, in your .vimrc add a line like `let g:coc_node_path = '/usr/local/opt/node@10/bin/node'`

## Why location list sometimes doesn't work?

Some plugin like [ale](https://github.com/w0rp/ale) would clear location list that created by other plugin, check out https://github.com/w0rp/ale/issues/1945, it's recommended to use `CocList diagnostics` to get all location list of diagnostics instead of using location list.

## No completion triggered after type trigger character sometimes.

Some language server could be slow for receiving document change before trigger completion, you can change the wait time for language server to finish the document change process before completion by change `coc.preferences.triggerCompletionWait` in your `coc-settings.json`, it's default to `60` in milliseconds.

## How could I add highlight to the markdown documentation?

Use a markdown plugin which could provide fency code highlight, like https://github.com/tpope/vim-markdown, and use settings like:

```
  let g:markdown_fenced_languages = ['css', 'javascript', 'js=javascript', 'typescript']
```
in your `.vimrc`.

## How to change highlight of diagnostic signs?

The sign highlight groups are `CocErrorSign` `CocWarningSign` `CocInfoSign` `CocHintSign`

You can customize the highlight by adding something like:

``` vim
highlight link CocErrorSign GruvboxRed
```
to your `.vimrc`.

See `:h coc-highlights` for all highlight groups.

## How to get log of coc.nvim?

Enable debug mode for coc in your terminal:

```
export NVIM_COC_LOG_LEVEL=debug
```

Open log file by command:
```
:CocOpenLog
```

The log would be cleared just after coc server started.