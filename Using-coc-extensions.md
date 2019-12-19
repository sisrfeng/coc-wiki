### Why are coc extensions needed?

The main reason is that some language servers provided by the community behave badly compared to the extensions of VSCode. Coc extensions can be forked from VSCode extensions providing a better user experience.

Compared to configured language servers, extensions have more features.

* Extensions can contribute properties to the schema `coc-settings.json`, like in VSCode you can write the configuration with completion and validation support when you have `coc-json` installed.

  <img width="561" alt="Screen Shot 2019-06-26 at 3 22 05 PM" src="https://user-images.githubusercontent.com/251450/60159618-32cd3800-9826-11e9-886e-b51fe07bf988.png">

* Extensions can contribute commands (like VSCode), you can use the coc commands in different ways:
  * Use the command `:CocList commands` to open the command list and choose one you need.

      <img width="476" alt="screen shot 2018-09-07 at 4 53 12 pm" src="https://user-images.githubusercontent.com/251450/45209334-4d4a1b00-b2bf-11e8-94e0-0c2b981a71f5.png">
  * Use `:CocCommand` with `<tab>` for command line completion.
  * An example config to use the custom command `Tsc` for `tsserver.watchBuild`:

    ```vim
    command! -nargs=0 Tsc    :CocCommand tsserver.watchBuild
    ```

* Extensions can contribute json schemas (same as VSCode)
* Extensions can contribute snippets that can be loaded by [coc-snippets](https://github.com/neoclide/coc-snippets) extension.
* Extensions can specify more client options, like `fileEvents` to watch files (requires [watchman](https://facebook.github.io/watchman/) installed), and `middleware` which can be used to fix results that return from the language server.

## Differences between coc extensions and VSCode extensions

* Coc extensions use [coc.nvim](https://www.npmjs.org/package/coc.nvim) as a dependency instead of [VSCode](https://www.npmjs.com/package/vscode)
* Coc extensions support language server features by using the API from coc.nvim instead of [vscode-languageclient](https://www.npmjs.com/package/vscode-languageclient) which can only be used with VSCode.
* Coc extensions support some features of VSCode extensions:
  * `activate` and `deactivate` api.
  * `activationEvents` in package.json.
  * Configuration support: `contributes.configuration` in package.json.
  * Commands support: `contributes.commands`.
  * JSON validation support via JSON Schema: `contributes.jsonValidation`.
  * Snippets support.

## Manage coc extensions

### Single file extensions

Coc.nvim will try to load javascript files in the folder `$VIMCONFIG/coc-extensions`, each javascript file should be extension of coc.nvim.

An example coc extension that convert character to unicode code point at cursor position.

``` js
const { commands, workspace } = require('coc.nvim')

exports.activate = context => {
  let { nvim } = workspace
  context.subscriptions.push(commands.registerCommand('code.convertCodePoint', async () => {
    let [pos, line] = await nvim.eval('[coc#util#cursor(), getline(".")]')
    let curr = pos[1] == 0 ? '' : line.slice(pos[1], pos[1] + 1)
    let code = curr.codePointAt(0)
    let str = code.toString(16)
    str = str.length == 4 ? str : '0'.repeat(4 - str.length) + str
    let result = `${line.slice(0, pos[1])}${'\\u' + str}${line.slice(pos[1] + 1)}`
    await nvim.call('setline', ['.', result])
  }))
}
```

**Note:** it's not possible to use package.json to add additional contributions for single file extensions.

### Install extensions

Using `:CocInstall`:

```vim
:CocInstall coc-json coc-css
```

One or more extension names can be provided.

The extension name can also be the url of a git repository, like: `https://github.com/andys8/vscode-jest-snippets.git#master` which could be accepted by `yarn install`.

Extensions will be loaded and activated after the install succeeds.

**Note** you can add extension names to the `g:coc_global_extensions` variable, and coc will install the missing extensions for you on server start.

To install extensions with shell script, use command like:

``` sh
# install coc-json & coc-html and exit
vim -c 'CocInstall -sync coc-json coc-html|q'
```

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

Use the command `:CocUpdate` or `:CocUpdateSync` to update all extensions to the latest version.

To upgrade extensions with shell script, use command like:

``` sh
vim -c 'CocUpdateSync|q'
```

### Uninstall coc extension

```vim
:CocUninstall coc-css
```

### Manage extensions with CocList

Run command:

```vim
CocList extensions
```

to open CocList buffer, which looks like:

<img width="619" alt="screen shot 2018-09-10 at 10 28 06 pm" src="https://user-images.githubusercontent.com/251450/45303659-e475d380-b548-11e8-9671-8a3e8e116db4.png">

* `?` means invalid extension
* `*` means extension is activated
* `+` means extension is loaded
* `-` means extension is disabled

Supported actions (hit tab to activate action menu):

* `toggle` default action. activate/deactivate selected extension(s).
* `enable`: enable selected extension(s).
* `disable`: disable selected extension(s).
* `reload`: reload selected extension(s).
* `uninstall`: remove selected extension(s).
* `lock`: toggle lock of extension, locked extension won't be updated by `:CocUpdate`

## Debug coc extension

If an extension throws uncaught errors, you can get the error message by: `:messages`.

For extensions using a language server, you can use the output channel. Check out <https://github.com/neoclide/coc.nvim/wiki/Debug-language-server#using-output-channel>.

If the extension is using stdio to write messages, you can get the output from the log file of coc, the log file can be found by command: `:CocOpenLog` in your vim. Use `$NVIM_COC_LOG_FILE` environment variable to use fixed filename for the log file.

The default log level is info. To get the debug information, set the `NVIM_COC_LOG_LEVEL` environment variable by the command: `export NVIM_COC_LOG_LEVEL=debug`.

You can also use chrome to debug extensions, checkout <https://github.com/neoclide/coc.nvim/wiki/Debug-coc.nvim>.

## Implemented coc extensions

You can find available coc extensions by searching [coc.nvim on npm](https://www.npmjs.com/search?q=keywords%3Acoc.nvim), or use [coc-marketplace](https://github.com/fannheyward/coc-marketplace), which can search and install extensions in coc.nvim directly.

* **[coc-json](https://github.com/neoclide/coc-json)** for `json`.
* **[coc-tsserver](https://github.com/neoclide/coc-tsserver)** for `javascript` and `typescript`.
* **[coc-html](https://github.com/neoclide/coc-html)** for `html`, `handlebars` and `razor`.
* **[coc-css](https://github.com/neoclide/coc-css)** for `css`, `scss` and `less`.
* **[coc-ember](https://github.com/NullVoxPopuli/coc-ember)** for ember projects.
* **[coc-vetur](https://github.com/neoclide/coc-vetur)** for `vue`, use [vetur](https://github.com/vuejs/vetur).
* **[coc-phpls](https://github.com/marlonfan/coc-phpls)** for `php`, use [intelephense-docs](https://github.com/bmewburn/intelephense-docs).
* **[coc-java](https://github.com/neoclide/coc-java)** for `java`, use [eclipse.jdt.ls](https://github.com/eclipse/eclipse.jdt.ls).
* **[coc-solargraph](https://github.com/neoclide/coc-solargraph)** for `ruby`, use [solargraph](http://solargraph.org/).
* **[coc-rls](https://github.com/neoclide/coc-rls)** for `rust`, use [Rust Language Server](https://github.com/rust-lang/rls)
* **[coc-rust-analyzer](https://github.com/fannheyward/coc-rust-analyzer)** for `rust`, use [rust-analyzer](https://github.com/rust-analyzer/rust-analyzer)
* **[coc-r-lsp](https://github.com/neoclide/coc-r-lsp)** for `r`, use [R languageserver](https://github.com/REditorSupport/languageserver).
* **[coc-yaml](https://github.com/neoclide/coc-yaml)** for `yaml`
* **[coc-python](https://github.com/neoclide/coc-python)** for `python`, extension forked from [vscode-python](https://github.com/Microsoft/vscode-python).
* **[coc-highlight](https://github.com/neoclide/coc-highlight)** provides default document symbol highlighting and color support.
* **[coc-emmet](https://github.com/neoclide/coc-emmet)** provides emmet suggestions in completion list.
* **[coc-snippets](https://github.com/neoclide/coc-snippets)** provides snippets solution.
* **[coc-lists](https://github.com/neoclide/coc-lists)** provides some basic lists like fzf.vim.
* **[coc-git](https://github.com/neoclide/coc-git)** provides git integration.
* **[coc-yank](https://github.com/neoclide/coc-yank)** provides yank highlights & history.
* **[coc-fsharp](https://github.com/yatli/coc-fsharp)** for `fsharp`.
* **[coc-svg](https://github.com/iamcco/coc-svg)** for `svg`.
* **[coc-tailwindcss](https://github.com/iamcco/coc-tailwindcss)** for `tailwindcss`.
* **[coc-angular](https://github.com/iamcco/coc-angular)** for `angular`.
* **[coc-vimlsp](https://github.com/iamcco/coc-vimlsp)** for `viml`.
* **[coc-xml](https://github.com/fannheyward/coc-xml)** for `xml`, use [lsp4xml](https://github.com/angelozerr/lsp4xml).
* **[coc-elixir](https://github.com/amiralies/coc-elixir)** for `elixir`, based on [elixir-ls](https://github.com/JakeBecker/elixir-ls/).
* **[coc-erlang_ls](https://github.com/hyhugh/coc-erlang_ls)** for `erlang`, based on [erlang_ls](https://github.com/erlang-ls/erlang_ls)
* **[coc-tabnine](https://github.com/neoclide/coc-tabnine)** for [tabnine](https://tabnine.com/).
* **[coc-powershell](https://github.com/yatli/coc-powershell)** for PowerShellEditorService integration.
* **[coc-omnisharp](https://github.com/yatli/coc-omnisharp)** for `csharp` and `visualbasic`.
* **[coc-texlab](https://github.com/fannheyward/coc-texlab)** for `LaTex` using [TexLab](https://texlab.netlify.com/).
* **[coc-lsp-wl](https://github.com/voldikss/coc-lsp-wl)** for `wolfram mathematica`, fork of [vscode-lsp-wl](https://github.com/kenkangxgwe/vscode-lsp-wl).
* **[coc-flow](https://github.com/amiralies/coc-flow)** for [`flow`](https://flow.org)
* **[coc-reason](https://github.com/jaredly/reason-language-server/tree/master/editor-extensions/coc.nvim)** for [`reasonml`](https://reasonml.github.io/)
* **[coc-svelte](https://github.com/coc-extensions/coc-svelte)** for [`svelte`](https://github.com/sveltejs/svelte)
* **[coc-flutter](https://github.com/iamcco/coc-flutter)** for [`flutter`](https://github.com/flutter/flutter)
* **[coc-pyright](https://github.com/fannheyward/coc-pyright)** [Pyright](https://github.com/microsoft/pyright) extension
* **[coc-markdownlint](https://github.com/fannheyward/coc-markdownlint)** for markdown linting
* **[coc-ecdict](https://github.com/fannheyward/coc-ecdict)** ECDICT extension
* **[coc-metals](https://github.com/ckipp01/coc-metals)** for Scala using Metals


**Tips:** use `:CocConfig` to edit the configuration file. Completion & validation are supported after `coc-json` is installed.
