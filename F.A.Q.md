Here's some common problems that you may need to understand when working with coc.nvim.

## How to install coc.nvim from master branch?

* Make user you don't have `{"tag": "*"}` in `Plug` command if you're using vim-plug as plugin manager.
* Use `Plug 'neoclide/coc.nvim', {'do': 'yarn install --frozen-lockfile'}` in your vimrc if you're using vim-plug.
* Add `let g:coc_force_debug = 1` to make sure coc use compiled code instead of binary.
* Update coc.nvim with `:PlugUpdate` command if you're using vim-plug, for other plugin manager run `:call coc#util#build()` after plugin update.
* Run `:echo coc#util#job_command()` to get command used for start coc.nvim service.
* Run `:checkhealth` when you have issue on neovim.

## How to make preview window shown aside with pum?

Build neovim from master code or [use nightly build](https://github.com/neovim/neovim/releases/tag/nightly).

To make sure floating preview window could work:

- `:echo exists('##MenuPopupChanged') && exists('*nvim_open_win')` should echo `1`.

**Note:** Floating preview window will not be shown when there're no detail and documentation with current complete item.

**Note:** To preview expanded snippet body, you can use [coc-snippets](https://github.com/neoclide/coc-snippets).

If you got errors including:

> Wrong number of arguments: expecting 5 but got 3.

It means you're using old version of neovim, upgrade to master to avoid this issue.

## My pum flick when typing.

It's much better with latest neovim, there is setting `suggest.reloadPumOnInsertChar`, when it's `true` the flick could be avoided on neovim, but it has bug with neovim's floating window, so not recommended.

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

If the message get shown after several seconds, add `set updatetime=300` to your `init.vim` or `.vimrc` and restart vim.

Some language server is known to have performance issue sometimes, including:

https://github.com/palantir/python-language-server

https://github.com/sourcegraph/go-langserver.

Don't report issue here when you found those language server is slow.

By default, coc doesn't show diagnostics in UI when you're in insert mode, 
add `"diagnostic.refreshOnInsertMode": true` to settings file to enable refresh on insert mode.

## Not working after upgrade node.

Run command `:CocRebuild` to rebuild coc extensions, some of them could be using C++ addons, which requires rebuild after upgrade.

If still not working, you can force coc started with specified node executable, in your .vimrc add a line like `let g:coc_node_path = '/usr/local/opt/node@10/bin/node'`

## Why location list sometimes doesn't work?

Some plugin like [ale](https://github.com/w0rp/ale) would clear location list that created by other plugin, check out https://github.com/w0rp/ale/issues/1945, it's recommended to use `CocList diagnostics` to get all location list of diagnostics instead of using location list.

## No completion triggered after type trigger character sometimes.

Some language server could be slow for receiving document change before trigger completion, you can change the wait time for language server to finish the document change process before completion by change `coc.preferences.triggerCompletionWait` in your `coc-settings.json`, it's default to `60` in milliseconds.

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