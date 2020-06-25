coc.nvim uses [jsonc](https://code.visualstudio.com/docs/languages/json) as configuration file format, the same as VSCode.
It's json that supports comments, like:

``` jsonc
{
  // my variable
  "foo": "bar"
}
```

To get correct comment highlighting, you can install [vim-jsonc](https://github.com/kevinoid/vim-jsonc) (which has built-in support for coc-settings.json), or even simply add:
``` vim
  autocmd FileType json syntax match Comment +\/\/.\+$+
```
to your `.vimrc` or `init.vim`.

## Opening the configuration file

Use the command `:CocConfig` to open your user configuration file, you can create a shortcut for the command like this:
``` vim
function! SetupCommandAbbrs(from, to)
  exec 'cnoreabbrev <expr> '.a:from
        \ .' ((getcmdtype() ==# ":" && getcmdline() ==# "'.a:from.'")'
        \ .'? ("'.a:to.'") : ("'.a:from.'"))'
endfunction

" Use C to open coc config
call SetupCommandAbbrs('C', 'CocConfig')
```

## Why use JSON?

* LSP uses JSON for language server configuration.
* Extensions can contribute JSON schema for json validation.
* [coc-json](https://github.com/neoclide/coc-json) can provide completion and validation for the settings file, which makes configuration much easier and more reliable.
* Most configurations take effect just after the settings file is saved, you don't need to restart vim or coc.

## Configuration file resolve

There're two types of user configuration files.

* The user configuration is named as `coc-settings.json` and placed inside the folder `$XDG_CONFIG_HOME/nvim` or `$HOME/.config/nvim` by default（or `$HOME/.vim` for vim). Run the command `:CocConfig` to open your user configuration file. 

* The workspace configuration should be named `coc-settings.json` and be in the directory `.vim`. 
After a file is opened in vim, this directory is resolved from the parent directories of that file. Run the command `:CocLocalConfig` to open your workspace configuration file. 

The active configuration is the merged result of the 'default', 'user' and 'workspace' configuration files, **the later one has the highest priority**.

To enable intellisense for `coc-settings.json`, install the json language extension [coc-json](https://github.com/neoclide/coc-json) with:
```
:CocInstall coc-json
```
in your vim.

## Default COC preferences

Checkout [schema.json](https://github.com/neoclide/coc.nvim/blob/master/data/schema.json)

**Note:** there are also some vim global variables used for configuration, check out `:h coc-variable`.

## Extension configuration

Just like VSCode, each coc extension can contribute configuration sections, for example:

* [coc-tsserver](https://github.com/neoclide/coc-tsserver) uses section `tsserver`, `typescript` and `javascript`
* [coc-json](https://github.com/neoclide/coc-json) uses section `json`

To get detailed options for existing configurations, just try the completion in the file `coc-settings.json`:

<img width="424" alt="screen shot 2018-07-13 at 2 17 26 pm" src="https://user-images.githubusercontent.com/251450/42675689-c9eb04e2-86a7-11e8-94b8-792f247a7394.png">