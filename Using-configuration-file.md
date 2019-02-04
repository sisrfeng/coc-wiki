coc.nvim use [jsonc](https://code.visualstudio.com/docs/languages/json) as configuration file format, the same as VSCode.
It's json that support comment, like:

``` jsonc
{
  // my variable
  "foo": "bar"
}
```

To get correct comment highlight, add:
``` vim
  autocmd FileType json syntax match Comment +\/\/.\+$+
```
to your `.vimrc` or `init.vim`.

## Why use JSON?

* LSP use JSON for language server configuration.
* Extensions could contribute JSON schema for json validation.
* [coc-json](https://github.com/neoclide/coc-json) could provide completion and validation for settings file, which makes configuration much easier and reliable.
* Most configurations would take effect just after settings file saved, you don't need to restart vim or coc.

## Configuration file resolve

There're two types of user configuration file.

* The user configuration is named as `coc-settings.json` and placed inside folder `$XDG_CONFIG_HOME/nvim` or `$HOME/.config/nvim` by default（or `$HOME/.vim` for vim）. Run command `:CocConfig` to open your user configuration file. 

* The workspace configuration should be named with `coc-settings.json` and would be resolve in directory `.vim`. 
After a file opened in vim, this directory would be resolved from parent directories of that file.

The active configuration would be a merged result from 'default', 'user' and 'workspace' configuration file, **the later one have higher priority**.

To enable intellisense of `coc-settings.json`, install json language extension [coc-json](https://github.com/neoclide/coc-json) by:
```
:CocInstall coc-json
```
in your vim.

## Default COC preferences

Checkout [schema.json](https://github.com/neoclide/coc.nvim/blob/master/data/schema.json)

**Note:** there're also some vim global variable used for configuration, check out `:h coc-variable`.

## Extension configuration

Just like VSCode, each coc extension could contribute configuration sections, for example.

* [coc-tsserver](https://github.com/neoclide/coc-tsserver) use section `tsserver`, `typescript` and `javascript`
* [coc-json](https://github.com/neoclide/coc-json) use section `json`

To get detailed options for existing configurations, just try completion in file `coc-settings.json`:

<img width="424" alt="screen shot 2018-07-13 at 2 17 26 pm" src="https://user-images.githubusercontent.com/251450/42675689-c9eb04e2-86a7-11e8-94b8-792f247a7394.png">