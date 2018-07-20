## Using output channel

Like VSCode, each language server have a output channel itself, the output channel could be opened by

```
:call CocAction('runCommand', 'workspace.showOutput')
```

Or
```
:Denite coc-command
```
to get command list and run command by type `<enter>`.

To make output channel track LSP communication, set `[languageserverId].trace.server` to `true` in your `coc-settings.json`.

For example, to make `cssserver` track LSP communication, use:
```
  "cssserver.trace.server": "verbose",
```

However, the output of LSP communication is difficult for human to read, you can upload the content to LSP inspector: https://microsoft.github.io/language-server-protocol/inspector/, which would be looks like:

<img width="833" alt="screen shot 2018-07-20 at 12 15 10 pm" src="https://user-images.githubusercontent.com/251450/42982989-c32a21d2-8c16-11e8-84ea-630497a24900.png">
