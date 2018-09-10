Start from coc 0.0.15, all extensions are separated from the core.

You **have to** install the extension you need by using vim command `CocInstall` like:

``` vim
:CocInstall coc-json coc-html coc-css
```

### Why should I use coc extensions?

Compare to the language server specified with `languageserver`, extensions have more features.

* Extensions could contribute properties to schema of `coc-settings.json`, like VSCode, you can write the configuration with completion support when you have `coc-json` installed.
    
  <img width="466" alt="screen shot 2018-09-07 at 5 03 24 pm" src="https://user-images.githubusercontent.com/251450/45209588-f5f87a80-b2bf-11e8-80c0-fe5ff689f947.png">

* Extensions could contribute commands (like VSCode), you can use the coc commands in different ways:
    * Use command `:Denite coc-command` to open the command list and choose one you need.

      <img width="476" alt="screen shot 2018-09-07 at 4 53 12 pm" src="https://user-images.githubusercontent.com/251450/45209334-4d4a1b00-b2bf-11e8-94e0-0c2b981a71f5.png">

    * Use a custom command to invoke the command, for exmaple, create a `Tsc` command for `tsserver.watchBuild` could be
        ```
        command! -nargs=0 Tsc    :call CocAction('runCommand', 'tsserver.watchBuild')
        ```
* Extensions could contribute json schemas (same as VSCode)
* Extensions could have specified more client options, like `fileEvents` to watch files (require [watchman](https://facebook.github.io/watchman/) installed), and `middleware` which could be used to fix the result that returned from language server.

## Differencse between coc extension and VSCode extension.

* Coc extensions use [coc.nvim](https://www.npmjs.org/package/coc.nvim) as dependency instead of [VSCode](https://www.npmjs.com/package/vscode)
* Coc extensions support language server features by using API from coc.nvim instead of [vscode-languageclient](https://www.npmjs.com/package/vscode-languageclient) which is bundled with VSCode.
* Coc extensions support some of features that VSCode extensions have:
  * `activate` and `deactivate` api.
  * `activationEvents` in package.json.
  * Configuration support: `contributes.configuration` in package.json.
  * Commands support: `contributes.commands`.

## Implemented coc extensions

* [coc-json](https://github.com/neoclide/coc-json): language server features for json.
* [coc-css](https://github.com/neoclide/coc-css): language server features for css, less, scss and wxss.
* [coc-eslint](https://github.com/neoclide/coc-eslint): language server using eslint for javascript files.
* [coc-html](https://github.com/neoclide/coc-html): language server features for html.
* [coc-pyls](https://github.com/neoclide/coc-pyls): language server features using [python-language-server](https://github.com/palantir/python-language-server).
* [coc-solargraph](https://github.com/neoclide/coc-solargraph): language server features using [solargraph](http://solargraph.org) for ruby.
* [coc-stylelint](https://github.com/neoclide/coc-stylelint): language server using stylelint for css files, just like [stylelint-vscode](https://github.com/shinnn/stylelint-vscode#readme)/
* [coc-tslint](https://github.com/neoclide/coc-tslint): language server using tslint for typescript files.
* [coc-vetur](https://github.com/neoclide/coc-vetur): language server features using [vue-language-server](https://www.npmjs.com/package/vue-language-server) for `vue` files.
* [coc-wxml](https://github.com/neoclide/coc-wxml): language server features for wxml.

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

## Manage extenions using denite

Denite interface could be used for manage extensions, if you have
[denite.nvim](https://github.com/Shougo/denite.nvim) installed.

Run command:
```
Denite coc-extension
```
to open denite buffer, which looks like:
<img width="619" alt="screen shot 2018-09-10 at 10 28 06 pm" src="https://user-images.githubusercontent.com/251450/45303659-e475d380-b548-11e8-9671-8a3e8e116db4.png">

`*` means extension is activited, `+` means loaded and `-` means disabled.

Supported actions:

* `toggle` default action. Enable/disable selected extension(s).
* `activate`: activate selected extension(s).
* `deactivate`: deactivate selected extension(s).
* `reload`: reload selected extension(s).
