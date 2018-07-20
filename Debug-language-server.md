# Contents

* [Using output channel](https://github.com/neoclide/coc.nvim/wiki/Debug-language-server#using-output-channel)
* [Using Chrome developer tools](https://github.com/neoclide/coc.nvim/wiki/Debug-language-server#using-chrome-developer-tools)

## Using output channel

The same as VSCode, each language server have a output channel itself, the output channel could be opened by

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

## Using Chrome developer tools

You can use Chrome to debug language server which using node IPC for communication.

First, add `execArgv` to the language server settings like:

```
 "cssserver.execArgv": ["--nolazy", "--inspect-brk=6045"]
```

After the `cssserver` started, open Chrome with url `chrome://inspect`

Make sure `Discover network targets` option is checked, and then you will the debugging target.