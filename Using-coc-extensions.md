**Note** start from coc 0.0.16, all extensions are separated from the core.

You **have to** install the extension you need by using vim command `CocInstall` like:

``` vim
:CocInstall coc-json coc-html coc-css
```

### Why should I use coc extensions?

Compare to the language server specified with `languageserver`, extensions have more features.

* Extensions could contribute properties to schema of `coc-settings.json`, like VSCode, you can write the configuration with completion support when you have `coc-json` installed.
    
  <img width="466" alt="screen shot 2018-09-07 at 5 03 24 pm" src="https://user-images.githubusercontent.com/251450/45209588-f5f87a80-b2bf-11e8-80c0-fe5ff689f947.png">

* Extensions could contribute commands (like VSCode), you can use the coc commands in different ways:
    * Use command `:Denite coc-command` to open the command list and choose one your want.
       <img width="476" alt="screen shot 2018-09-07 at 4 53 12 pm" src="https://user-images.githubusercontent.com/251450/45209334-4d4a1b00-b2bf-11e8-94e0-0c2b981a71f5.png">
    * Use a custom command to invoke the command, for exmaple, create a `Tsc` command for `tsserver.watchBuild` could be
        ```
        command! -nargs=0 Tsc    :call CocAction('runCommand', 'tsserver.watchBuild')
        ```
* Extensions could have specify more client options, like `fileEvents` to watch files (require [watchman](https://facebook.github.io/watchman/) installed), and `middleware` which could be used to fix the result that returned from language server.

## Install/update coc extension

Use command `:CocInstall` like:

```
:CocInstall coc-json coc-css
```
One or more plugin name could be provided.

The extension would be loaded and activated after install succeed.

## Update all coc extensions

```
:CocUpdate
```

## Disable coc extension

More extension provide an `enable` option. 

For example, to disable `coc-json`, Open your config file by `:CocConfig` and add:

```
"json.enable": false
```

Run `:CocRestart` or restart your vim to make sure the change take effect.