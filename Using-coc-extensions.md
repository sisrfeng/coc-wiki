### Why coc extensions is needed?

The main reason is that some language servers provided by community behavior badly compare to extensions of VSCode, coc extensions could be forked from VSCode extensions to provide best user experience. 

Compare to the language server, extensions have more features.

* Extensions could contribute properties to schema of `coc-settings.json`, like VSCode, you can write the configuration with completion and validation support when you have `coc-json` installed.
    
  <img width="466" alt="screen shot 2018-09-07 at 5 03 24 pm" src="https://user-images.githubusercontent.com/251450/45209588-f5f87a80-b2bf-11e8-80c0-fe5ff689f947.png">

* Extensions could contribute commands (like VSCode), you can use the coc commands in different ways:
    * Use command `:CocList commands` to open the command list and choose one you need.

      <img width="476" alt="screen shot 2018-09-07 at 4 53 12 pm" src="https://user-images.githubusercontent.com/251450/45209334-4d4a1b00-b2bf-11e8-94e0-0c2b981a71f5.png">
    * Use `:CocCommand` with `<tab>` for command line completion.
    * Use a custom command to invoke the command, for exmaple, create a `Tsc` command for `tsserver.watchBuild` could be
        ```
        command! -nargs=0 Tsc    :CocCommand tsserver.watchBuild
        ```

* Extensions could contribute json schemas (same as VSCode)
* Extensions could contribute snippets that can be loaded by [coc-snippets](https://github.com/neoclide/coc-snippets) extension.
* Extensions could specify more client options, like `fileEvents` to watch files (require [watchman](https://facebook.github.io/watchman/) installed), and `middleware` which could be used to fix the result that returned from language server.

## Differences between coc extension and VSCode extension.

* Coc extensions use [coc.nvim](https://www.npmjs.org/package/coc.nvim) as dependency instead of [VSCode](https://www.npmjs.com/package/vscode)
* Coc extensions support language server features by using API from coc.nvim instead of [vscode-languageclient](https://www.npmjs.com/package/vscode-languageclient) which could only be used with VSCode.
* Coc extensions support some features of VSCode extensions:
  * `activate` and `deactivate` api.
  * `activationEvents` in package.json.
  * Configuration support: `contributes.configuration` in package.json.
  * Commands support: `contributes.commands`.
  * Json shemas assosication: `contributes.jsonValidation`.
  * Snippets support.

## Manage coc extensions

### Install extensions

Use command `:CocInstall` like:

```
:CocInstall coc-json coc-css
```
One or more extension name could be provided.

The extension name could also be url of git repository, like: `https://github.com/andys8/vscode-jest-snippets.git#master` which could be accepted by `yarn install`.

Extensions would be loaded and activated after install succeed.

**Note** you can add extension names to `g:coc_global_extensions` variable, coc would install the missing extensions for you on server start.

### Use vim's plugin manager for coc extension

Start from recent master of coc.nvim, you can manage coc extension by use plugin manager for vim, like [vim-plug](https://github.com/junegunn/vim-plug), coc would try to load coc extensions from your `&rtp`, for example, install coc-tsserver would be add:

``` vim
Plug 'neoclide/coc-tsserver', {'do': 'yarn install --frozen-lockfile'}
```

to your vimrc, and run `PlugInstall`, the limitation is your can't uninstall the extension by use `:CocUninstall` and automatic update support is not available.

### Update extensions

You **don't** need to update coc extensions manually, coc detect acceptable new version of installed extension everyday(by default) the first time it started, when it found new version of extensions, it update them for you automatically.

To disable automatic update, change settings: `coc.preferences.extensionUpdateCheck` to `"never"`.

Use command `:CocUpdate` or `:CocUpdateSync` to update all extensions to latest version, the upgrade won't work if you're not using latest version of coc.nvim to avoid possible break change.

## Uninstall coc extension

```
:CocUninstall coc-css
```

## Manage extenions using CocList

Run command:
```
CocList extensions
```

to open CocList buffer, which looks like:

<img width="619" alt="screen shot 2018-09-10 at 10 28 06 pm" src="https://user-images.githubusercontent.com/251450/45303659-e475d380-b548-11e8-9671-8a3e8e116db4.png">

`?` means invalid extension, `*` means extension is activited, `+` means loaded and `-` means disabled

Supported actions:

* `toggle` default action. activate/deactivate selected extension(s).
* `enable`: enable selected extension(s).
* `disable`: disable selected extension(s).
* `reload`: reload selected extension(s).
* `uninstall`: remove selected extension(s).

## Debug coc extension

If extension throw uncaught errors, you can get the error message by: `:messages`.

For extension using language server, you can use output channel, check out https://github.com/neoclide/coc.nvim/wiki/Debug-language-server#using-output-channel.

If the extension using stdio to write messages, you can get the output from the log file of coc, the log file could be find by run command: `node -e 'console.log(path.join(os.tmpdir(), "coc-nvim.log"))'`.

The default log level is info, to get the debug information, set `NVIM_COC_LOG_LEVEL` environment variable by command: `export NVIM_COC_LOG_LEVEL=debug`.

## Implemented coc extensions

https://www.npmjs.com/search?q=keywords%3Acoc.nvim
