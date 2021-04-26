### Why are coc extensions needed?

The main reason is that some language servers provided by the community behave badly compared to the extensions of VSCode. Coc extensions can be forked from VSCode extensions providing a better user experience.

Compare to configured language servers, extensions have more features.

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

For a deeper dive into the purpose and implementation of coc extensions, please see [How to write a coc.nvim extension](https://samroeca.com/coc-plugin.html#writing-an-extension).

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

**Note:** VSCode extensions can't be used by coc.nvim for now. 

Extensions will be loaded and activated after the install succeeds.

**Note** you can add extension names to the `g:coc_global_extensions` variable, and coc will install the missing extensions after coc.nvim service started.
For example:
```vim
let g:coc_global_extensions = ['coc-json', 'coc-git']
```

To install extensions with shell script, use command like:

``` sh
# install coc-json & coc-html and exit
vim -c 'CocInstall -sync coc-json coc-html|q'
```

#### Installing specific versions (for rollback/revert/etc)
If you need to rollback/revert to a specific extension version, or you just want to install a specific version regardless, you can add `@version` to your install command.

Using `coc-prettier` as an example, in order to install the `1.1.17` version, you type:
```vim
:CocInstall coc-prettier@1.1.17
```

### Use vim's plugin manager for coc extension

You can manage coc extension by using a plugin manager for vim, like [vim-plug](https://github.com/junegunn/vim-plug), coc will try to load coc extensions from your `&rtp`

Example for coc-tsserver:

``` vim
Plug 'neoclide/coc-tsserver', {'do': 'yarn install --frozen-lockfile'}
```

After adding this to your vimrc run `PlugInstall`. 

**Note:** For coc extensions written with typescript, you have to build them when using git, most of time you should install [yarn](https://yarnpkg.com) and use `yarn install --frozen-lockfile` in extension root.

This has the limitation that you can't uninstall the extension by using `:CocUninstall` and that automatic update support is not available.

### Update extensions

Use the command `:CocUpdate` or `:CocUpdateSync` to update all extensions to the latest version.

You can enable automatic update by configuration `coc.preferences.extensionUpdateCheck` to `"daily"`, which defaults to `"never"`.

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
:CocList extensions
```

to open CocList buffer, which looks like:

<img width="619" alt="screen shot 2018-09-10 at 10 28 06 pm" src="https://user-images.githubusercontent.com/251450/45303659-e475d380-b548-11e8-9671-8a3e8e116db4.png">

* `?` means invalid extension
* `*` means extension is activated
* `+` means extension is loaded
* `-` means extension is disabled

Supported actions (hit `<Tab>` to activate action menu):

* `toggle` default action. activate/deactivate selected extension(s).
* `enable`: enable selected extension(s).
* `disable`: disable selected extension(s).
* `reload`: reload selected extension(s).
* `uninstall`: remove selected extension(s).
* `lock`: toggle lock of extension, locked extension won't be updated by `:CocUpdate`

## Debug coc extension

If an extension throws uncaught errors, you can get the error message by: `:messages`.

For extensions using a language server, you can use the output channel. Check out <https://github.com/neoclide/coc.nvim/wiki/Debug-language-server#using-output-channel>.

Use `console` for log messages to log file of coc.nvim, supported methods including `debug` `log` `error` `info` `warn`, checkout `:h :CocOpenLog`. 

You can also use Chrome to debug extensions, checkout <https://github.com/neoclide/coc.nvim/wiki/Debug-coc.nvim>.

## Implemented coc extensions

You can find available coc extensions by searching [coc.nvim on npm](https://www.npmjs.com/search?q=keywords%3Acoc.nvim), or use [coc-marketplace](https://github.com/fannheyward/coc-marketplace), which can search and install extensions in coc.nvim directly.

* **[coc-angular](https://github.com/iamcco/coc-angular)** for `angular`.
* **[coc-blade-formatter](https://github.com/yaegassy/coc-blade-formatter)** for `blade`, Integrates the [blade-formatter](https://github.com/shufo/blade-formatter) (Laravel Blade formatter).
* **[coc-blade-linter](https://github.com/yaegassy/coc-blade-linter)** for `blade`, Integrates the [Laravel Blade Linter](https://github.com/bdelespierre/laravel-blade-linter).
* **[coc-browser](https://github.com/voldikss/coc-browser)** for browser words completion
* **[coc-calc](https://github.com/weirongxu/coc-calc)** expression calculation extension
* **[coc-cfn-lint](https://github.com/joenye/coc-cfn-lint)** for CloudFormation Linter, [cfn-python-lint](https://github.com/aws-cloudformation/cfn-python-lint)
* **[coc-clangd](https://github.com/clangd/coc-clangd)** for C/C++/Objective-C, use [clangd](https://clangd.github.io)
* **[coc-clang-format-style-options](https://www.npmjs.com/package/coc-clang-format-style-options)** coc.nvim extension, helps you write `.clang-format` more easily.
* **[coc-cmake](https://github.com/voldikss/coc-cmake)** for cmake code completion
* **[coc-css](https://github.com/neoclide/coc-css)** for `css`, `scss` and `less`.
* **[coc-cssmodules](https://github.com/antonk52/coc-cssmodules)** css modules intellisense.
* **[coc-deno](https://github.com/fannheyward/coc-deno)** for [deno](https://github.com/denoland/deno).
* **[coc-denoland](https://github.com/LumaKernel/coc-denoland)** for [deno](https://github.com/denoland/deno), fork of [vscode_deno](https://github.com/denoland/vscode_deno).
* **[coc-diagnostic](https://github.com/iamcco/coc-diagnostic)** for All filetypes, use [diagnostic-languageserver](https://github.com/iamcco/diagnostic-languageserver).
* **[coc-discord](https://github.com/amiralies/coc-discord)** discord rich presence for coc.nvim
* **[coc-discord-rpc](https://github.com/LeonardSSH/coc-discord-rpc)** fully customizable discord rpc integration with support for over 130+ of the most popular languages
* **[coc-dash-complete](https://github.com/voldikss/coc-dash-complete)** Press `-` to trigger buffer source completion.
* **[coc-dot-complete](https://github.com/voldikss/coc-dot-complete)** Press `.` to trigger buffer source completion.
* **[coc-ecdict](https://github.com/fannheyward/coc-ecdict)** ECDICT extension
* **[coc-elixir](https://github.com/elixir-lsp/coc-elixir)** for `elixir`, based on [elixir-ls](https://github.com/elixir-lsp/elixir-ls/).
* **[coc-ember](https://github.com/NullVoxPopuli/coc-ember)** for ember projects.
* **[coc-emmet](https://github.com/neoclide/coc-emmet)** provides emmet suggestions in completion list.
* **[coc-erlang_ls](https://github.com/hyhugh/coc-erlang_ls)** for `erlang`, based on [erlang_ls](https://github.com/erlang-ls/erlang_ls)
* **[coc-esbonio](https://github.com/yaegassy/coc-esbonio)** for `rst` (reStructuredText), [esbonio](https://pypi.org/project/esbonio/) ([Sphinx] Python Documentation Generator) language server extension.
* **[coc-eslint](https://github.com/neoclide/coc-eslint)** Eslint extension for coc.nvim
* **[coc-explorer](https://github.com/weirongxu/coc-explorer)** file explorer extension
* **[coc-floaterm](https://github.com/voldikss/coc-floaterm)** for [vim-floaterm](https://github.com/voldikss/vim-floaterm) integration
* **[coc-flow](https://github.com/amiralies/coc-flow)** for [`flow`](https://flow.org)
* **[coc-flutter](https://github.com/iamcco/coc-flutter)** for [`flutter`](https://github.com/flutter/flutter)
* **[coc-fsharp](https://github.com/yatli/coc-fsharp)** for `fsharp`.
* **[coc-fzf-preview](https://github.com/yuki-ycino/fzf-preview.vim/)** provide powerful [fzf](https://github.com/junegunn/fzf) integration.
* **[coc-gist](https://github.com/voldikss/coc-gist)** gist management
* **[coc-git](https://github.com/neoclide/coc-git)** provides git integration.
* **[coc-glslx](https://github.com/Eric-Song-Nop/coc-glslx)** for `glsl`, use [glslx](https://github.com/evanw/glslx).
* **[coc-go](https://github.com/josa42/coc-go)** for `go`, use [gopls](https://github.com/golang/tools/tree/master/gopls).
* **[coc-graphql](https://github.com/felippepuhle/coc-graphql)** for `graphql`.
* **[coc-highlight](https://github.com/neoclide/coc-highlight)** provides default document symbol highlighting and color support.
* **[coc-html](https://github.com/neoclide/coc-html)** for `html`, `handlebars` and `razor`.
* **[coc-htmldjango](https://github.com/yaegassy/coc-htmldjango)** for `htmldjango`, django templates (htmldjango) extension. Provides "formatter", "snippets completion" and more...
* **[coc-htmlhint](https://github.com/yaegassy/coc-htmlhint)** for `html`, Integrates the HTMLHint static analysis tool.
* **[coc-html-css-support](https://github.com/yaegassy/coc-html-css-support)** for HTML id and class attribute completion.
* **[coc-intelephense](https://github.com/yaegassy/coc-intelephense)** for php, fork of [vscode-intelephense](https://github.com/bmewburn/vscode-intelephense). (scoped packages: `@yaegassy/coc-intelephense`)
* **[coc-java](https://github.com/neoclide/coc-java)** for `java`, use [eclipse.jdt.ls](https://github.com/eclipse/eclipse.jdt.ls).
* **[coc-jedi](https://github.com/pappasam/coc-jedi)** for `python`, use [jedi-language-server](https://github.com/pappasam/jedi-language-server).
* **[coc-json](https://github.com/neoclide/coc-json)** for `json`.
* **[coc-julia](https://github.com/fannheyward/coc-julia)** for [`julia`](https://julialang.org/).
* **[coc-just-complete](https://github.com/voldikss/coc-just-complete)** Press `_` to trigger buffer source completion.
* **[coc-lists](https://github.com/neoclide/coc-lists)** provides some basic lists like fzf.vim.
* **[coc-lsp-wl](https://github.com/voldikss/coc-lsp-wl)** for `wolfram mathematica`, fork of [vscode-lsp-wl](https://github.com/kenkangxgwe/vscode-lsp-wl).
* **[coc-markdownlint](https://github.com/fannheyward/coc-markdownlint)** for markdown linting
* **[coc-metals](https://github.com/scalameta/coc-metals)** for Scala using [`Metals`](http://scalameta.org/metals/)
* **[coc-omnisharp](https://github.com/yatli/coc-omnisharp)** for `csharp` and `visualbasic`.
* **[coc-perl](https://github.com/ryuta69/coc-perl)** for `perl`.
* **[coc-php-cs-fixer](https://github.com/yaegassy/coc-php-cs-fixer)** for `php`, Integrates the [php-cs-fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer) (PHP Coding Standards Fixer).
* **[coc-phpactor](https://github.com/phpactor/coc-phpactor)** for `php`, using [phpactor](https://github.com/phpactor/phpactor)
* **[coc-phpls](https://github.com/marlonfan/coc-phpls)** for `php`, use [intelephense-docs](https://github.com/bmewburn/intelephense-docs).
* **[coc-psalm](https://github.com/yaegassy/coc-psalm)** for `php`, use [psalm](https://psalm.dev/).
* **[coc-powershell](https://github.com/yatli/coc-powershell)** for PowerShellEditorService integration.
* **[coc-prettier](https://github.com/neoclide/coc-prettier)** a fork of [prettier-vscode](https://github.com/prettier/prettier-vscode).
* **[coc-prisma](https://github.com/pantharshit00/coc-prisma)** for Prisma schema integration.
* **[coc-pyright](https://github.com/fannheyward/coc-pyright)** [Pyright](https://github.com/microsoft/pyright) extension
* **[coc-python](https://github.com/neoclide/coc-python)** for `python`, extension forked from [vscode-python](https://github.com/Microsoft/vscode-python).
* **[coc-pydocstring](https://github.com/yaegassy/coc-pydocstring)** for `python`, using [doq](https://pypi.org/project/doq/) (python docstring generator) extension.
* **[coc-r-lsp](https://github.com/neoclide/coc-r-lsp)** for `r`, use [R languageserver](https://github.com/REditorSupport/languageserver).
* **[coc-reason](https://github.com/jaredly/reason-language-server/tree/master/editor-extensions/coc.nvim)** for [`reasonml`](https://reasonml.github.io/)
* **[coc-rls](https://github.com/neoclide/coc-rls)** for `rust`, use [Rust Language Server](https://github.com/rust-lang/rls)
* **[coc-rome](https://github.com/fannheyward/coc-rome)** for `javascript`, `typescript`, `json` and more, use [`Rome`](https://github.com/romefrontend/rome)
* **[coc-rust-analyzer](https://github.com/fannheyward/coc-rust-analyzer)** for `rust`, use [rust-analyzer](https://github.com/rust-analyzer/rust-analyzer)
* **[coc-sh](https://github.com/josa42/coc-sh)** for `bash` using [bash-language-server](https://github.com/bash-lsp/bash-language-server).
* **[coc-stylelintplus](https://github.com/bmatcuk/coc-stylelintplus)** for linting CSS and CSS preprocessed formats
* **[coc-stylelint](https://github.com/neoclide/coc-stylelint)** for linting CSS and CSS preprocessed formats
* **[coc-snippets](https://github.com/neoclide/coc-snippets)** provides snippets solution.
* **[coc-solargraph](https://github.com/neoclide/coc-solargraph)** for `ruby`, use [solargraph](http://solargraph.org/).
* **[coc-sourcekit](https://github.com/klaaspieter/coc-sourcekit)** for [`Swift`](https://swift.org/)
* **[coc-spell-checker](https://github.com/iamcco/coc-spell-checker)** A basic spell checker that works well with camelCase code
* **[coc-sql](https://github.com/fannheyward/coc-sql)** for `sql`.
* **[coc-svelte](https://github.com/coc-extensions/coc-svelte)** for [`svelte`](https://github.com/sveltejs/svelte).
* **[coc-svg](https://github.com/iamcco/coc-svg)** for `svg`.
* **[coc-swagger](https://github.com/haishanh/coc-swagger)** for improved Swagger/OpenAPI spec authoring experience.
* **[coc-tabnine](https://github.com/neoclide/coc-tabnine)** for [tabnine](https://tabnine.com/).
* **[coc-tailwindcss](https://github.com/iamcco/coc-tailwindcss)** for `tailwindcss`.
* **[coc-tasks](https://github.com/voldikss/coc-tasks)** for [asynctasks.vim](https://github.com/skywind3000/asynctasks.vim) integration
* **[coc-texlab](https://github.com/fannheyward/coc-texlab)** for `LaTex` using [TexLab](https://texlab.netlify.com/).
* **[coc-toml](https://github.com/kkiyama117/coc-toml)** for [`toml`](https://github.com/toml-lang/toml) using [taplo](https://github.com/tamasfe/taplo).
* **[coc-translator](https://github.com/voldikss/coc-translator)** language transaction extension
* **[coc-tsserver](https://github.com/neoclide/coc-tsserver)** for `javascript` and `typescript`.
* **[coc-vetur](https://github.com/neoclide/coc-vetur)** for `vue`, use [vetur](https://github.com/vuejs/vetur).
* **[coc-vimlsp](https://github.com/iamcco/coc-vimlsp)** for `viml`.
* **[coc-xml](https://github.com/fannheyward/coc-xml)** for `xml`, use [lsp4xml](https://github.com/angelozerr/lsp4xml).
* **[coc-yaml](https://github.com/neoclide/coc-yaml)** for `yaml`
* **[coc-yank](https://github.com/neoclide/coc-yank)** provides yank highlights & history.

**Tips:** use `:CocConfig` to edit the configuration file. Completion & validation are supported after `coc-json` is installed.
