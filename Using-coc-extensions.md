### Why are coc extensions needed?

The main reason is that some language servers provided by the community behave badly compared to the extensions of VSCode. Coc extensions can be forked from VSCode extensions providing a better user experience. 

Compared to configured language servers, extensions have more features.

* Extensions can contribute properties to the schema `coc-settings.json`, like in VSCode you can write the configuration with completion and validation support yourself when you have `coc-json` installed.
    
  <img width="466" alt="screen shot 2018-09-07 at 5 03 24 pm" src="https://user-images.githubusercontent.com/251450/45209588-f5f87a80-b2bf-11e8-80c0-fe5ff689f947.png">

* Extensions can contribute commands (like VSCode), you can use the coc commands in different ways:
    * Use the command `:CocList commands` to open the command list and choose one you need.

      <img width="476" alt="screen shot 2018-09-07 at 4 53 12 pm" src="https://user-images.githubusercontent.com/251450/45209334-4d4a1b00-b2bf-11e8-94e0-0c2b981a71f5.png">
    * Use `:CocCommand` with `<tab>` for command line completion.
    * An example config to use the custom command `Tsc` for `tsserver.watchBuild`:
        ```
        command! -nargs=0 Tsc    :CocCommand tsserver.watchBuild
        ```

* Extensions can contribute json schemas (same as VSCode)
* Extensions can contribute snippets that can be loaded by [coc-snippets](https://github.com/neoclide/coc-snippets) extension.
* Extensions can specify more client options, like `fileEvents` to watch files (requires [watchman](https://facebook.github.io/watchman/) installed), and `middleware` which can be used to fix results that return from the language server.

## Differences between coc extensions and VSCode extensions.

* Coc extensions use [coc.nvim](https://www.npmjs.org/package/coc.nvim) as a dependency instead of [VSCode](https://www.npmjs.com/package/vscode)
* Coc extensions support language server features by using the API from coc.nvim instead of [vscode-languageclient](https://www.npmjs.com/package/vscode-languageclient) which can only be used with VSCode.
* Coc extensions support some features of VSCode extensions:
  * `activate` and `deactivate` api.
  * `activationEvents` in package.json.
  * Configuration support: `contributes.configuration` in package.json.
  * Commands support: `contributes.commands`.
  * Json shemas assosication: `contributes.jsonValidation`.
  * Snippets support.

## Manage coc extensions

### Install extensions

Using `:CocInstall`:

```
:CocInstall coc-json coc-css
```
One or more extension names can be provided.

The extension name can also be the url of a git repository, like: `https://github.com/andys8/vscode-jest-snippets.git#master` which could be accepted by `yarn install`.

Extensions will be loaded and activated after the install succeeds.

**Note** you can add extension names to the `g:coc_global_extensions` variable, and coc will install the missing extensions for you on server start.

### Use vim's plugin manager for coc extension

Starting from recent master of coc.nvim, you can manage coc extension by using a plugin manager for vim, like [vim-plug](https://github.com/junegunn/vim-plug), coc will try to load coc extensions from your `&rtp`

Example for coc-tsserver:

``` vim
Plug 'neoclide/coc-tsserver', {'do': 'yarn install --frozen-lockfile'}
```

After adding this to your vimrc run `PlugInstall`. This has the limitation that you can't uninstall the extension by using `:CocUninstall` and that automatic update support is not available.

### Update extensions

You **don't** need to update coc extensions manually, coc detects acceptable new version of installed extension everyday (by default) the first time it starts. When it finds a new version of an extension, it will update it for you automatically.

To disable automatic updates, change the setting: `coc.preferences.extensionUpdateCheck` to `"never"`.

Use the command `:CocUpdate` or `:CocUpdateSync` to update all extensions to the latest version, the upgrade won't work if you're not using latest release version of coc.nvim to avoid possible breaking changes.

## Uninstall coc extension

```
:CocUninstall coc-css
```

## Manage extensions using CocList

Run command:
```
CocList extensions
```

to open CocList buffer, which looks like:

<img width="619" alt="screen shot 2018-09-10 at 10 28 06 pm" src="https://user-images.githubusercontent.com/251450/45303659-e475d380-b548-11e8-9671-8a3e8e116db4.png">

- `?` means invalid extension
- `*` means extension is activated
- `+` means extension is loaded
- `-` means extension is disabled

Supported actions:

* `toggle` default action. activate/deactivate selected extension(s).
* `enable`: enable selected extension(s).
* `disable`: disable selected extension(s).
* `reload`: reload selected extension(s).
* `uninstall`: remove selected extension(s).

## Debug coc extension

If an extension throws uncaught errors, you can get the error message by: `:messages`.

For extensions using a language server, you can use the output channel. Check out https://github.com/neoclide/coc.nvim/wiki/Debug-language-server#using-output-channel.

If the extension is using stdio to write messages, you can get the output from the log file of coc, the log file can be found by runnung the command: `node -e 'console.log(path.join(os.tmpdir(), "coc-nvim.log"))'`.

The default log level is info. To get the debug information, set the `NVIM_COC_LOG_LEVEL` environment variable by the command: `export NVIM_COC_LOG_LEVEL=debug`.

You can also use chrome to debug extensions, checkout https://github.com/neoclide/coc.nvim/wiki/Debug-coc.nvim.

## Implemented coc extensions

https://www.npmjs.com/search?q=keywords%3Acoc.nvim
