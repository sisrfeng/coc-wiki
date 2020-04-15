# Contents

* [Checkout server stats](https://github.com/neoclide/coc.nvim/wiki/Debug-language-server#checkout-server-stats)
* [Using output channel](https://github.com/neoclide/coc.nvim/wiki/Debug-language-server#using-output-channel)
* [Using Chrome developer tools](https://github.com/neoclide/coc.nvim/wiki/Debug-language-server#using-chrome-developer-tools)

## Checkout server stats

Use command `:CocList services` to open services list, you will have `id` `state` and `filetypes` for each service.

<img width="893" alt="Screen Shot 2020-04-15 at 4 45 00 PM" src="https://user-images.githubusercontent.com/251450/79318320-6609c080-7f39-11ea-9cfe-9921584a96d9.png">

If service id starts with `languageserver`, it comes from `languageserver` configuration in coc-settings.json, if not, it's from extension of coc.nvim.

## Using output channel

The same as VSCode, each language server has it's own output channel, the output channel can be opened by

```
:CocCommand workspace.showOutput
```

To make an output channel track all LSP communication, set `[languageserverId].trace.server` to `verbose` in your `coc-settings.json`.

For example, to make `tsserver` track LSP communication, use:

``` json
  "tsserver.trace.server": "verbose",
```

to make a custom language server track LSP communication, add a `trace.server` section in your language server configuration, like:

``` json
"languageserver":{
   "ccls": {
    "command": "ccls",
    "filetypes": ["c", "cpp", "objc", "objcpp"],
    "trace.server": "verbose",
    "initializationOptions": {
      "cacheDirectory": "/tmp/ccls"
    }
  }
}
```

However, the output of LSP communication is difficult for humans to read. You can upload the content to the LSP inspector: https://microsoft.github.io/language-server-protocol/inspector/, which looks like this:

<img width="833" alt="screen shot 2018-07-20 at 12 15 10 pm" src="https://user-images.githubusercontent.com/251450/42982989-c32a21d2-8c16-11e8-84ea-630497a24900.png">

## Using Chrome developer tools

You can use Chrome to debug a language server which is using node IPC for communication.

First, add `execArgv` to the language server settings like:

```
 "css.execArgv": ["--nolazy", "--inspect-brk=6045"]
```

After the `css` service starts, open Chrome with the url `chrome://inspect`

Make sure the `Discover network targets` option is checked and you have the address added to `Target discovery settings`, and then you will have the debugging target.