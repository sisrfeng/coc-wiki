Here's some common problems that you may need to understand when working with coc.nvim.

## Not working after upgrade node.

Run command `:CocRebuild` to rebuild coc extensions, some of them could be using C++ addons, which requires rebuild after upgrade.

If still not working, you can force coc started with specified node executable, in your .vimrc add a line like `let g:coc_node_path = '/usr/local/opt/node@10/bin/node'`

## Why location list sometimes doesn't work?

Some plugin like [ale](https://github.com/w0rp/ale) would clear location list that created by other plugin, check out https://github.com/w0rp/ale/issues/1945, it's recommended to use Denite coc-diagnostic to get all location list of diagnostics instead of using location list.

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