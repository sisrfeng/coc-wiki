COC.nvim use [jsonc](https://code.visualstudio.com/docs/languages/json) as configuration file format, the same as VSCode.
It's json that support comment, like:

``` jsonc
{
  // my variable
  "foo": "bar"
}
```

You can make use of [jsonc.vim](https://github.com/neoclide/jsonc.vim) to get correct highlight for comment.

# Contents

* [Configuration file resolve](#configuration-file-resolve)
* [Default COC preferences](#default-coc-preferences)
* [Extension configuration](#extension-configuration)

## Configuration file resolve

Same as VSCode settings file, there're three types of configuration file for COC.

* The global one bundles with this plugin, which is [schema.json](https://github.com/neoclide/coc.nvim/blob/master/data/schema.json)

* The user configuration is named as `coc-settings.json` and placed inside folder `$XDG_CONFIG_HOME/nvim` or `$HOME/.config/nvim` by default. Run command `:CocConfig` to open your user configuration file. 

* The workspace configuration should be named with `coc-settings.json` and would be resolve in directory `.vim`. 
After a file opened in vim, this directory would be resolved from parent directory of file. If the directory is not resolved for you, try restart coc server by command `:CocRestart`.

The active configuration would be a merged result from 'default', 'user' and 'workspace' configuration file, **the later one have higher priority**.

To enable intellisense of `coc-settings.json`, install json language extension [coc-json](https://github.com/neoclide/coc-json) by:
```
:CocInstall coc-json
```
in your vim.

## Default COC preferences

``` jsonc
  // could be 'always' 'trigger' => for specified trigger characters only 'none'
  "coc.preferences.autoTrigger": "always",
  // only used when autoTrigger is always
  "coc.preferences.triggerAfterInsertEnter": false,
  // timeout for completion
  "coc.preferences.timeout": 2000,
  // not make vim select first item on completion start
  "coc.preferences.noselect": true,
  // enable formatOnType feature
  "coc.preferences.formatOnType": false,
  // command used for jump to other buffer
  "coc.preferences.jumpCommand": "edit",
  // executable path for https://facebook.github.io/watchman/, detected from $PATH by default
  // "coc.preferences.watchmanPath": "",
  // enable diagnostic
  "coc.preferences.diagnostic.enable": true,
  "coc.preferences.diagnostic.signOffset": 1000,
  "coc.preferences.diagnostic.errorSign": ">>",
  "coc.preferences.diagnostic.warningSign": "⚠",
  "coc.preferences.diagnostic.infoSign": ">>",
  "coc.preferences.diagnostic.hintSign": ">>",
``` 

To get complete list, checkout [settings.json](https://github.com/neoclide/coc.nvim/blob/master/settings.json)

**Note:** for configure keymaps of coc, vim global variable is used, check out `coc-variable` section at `doc/coc.txt`.

## Extension configuration

Just like VSCode, each coc extension could contribute configuration sections, for example.

* [coc-tsserver](https://github.com/neoclide/coc-tsserver) use section `tsserver`, `typescript` and `javascript`
* [coc-json](https://github.com/neoclide/coc-json) use section `json` and `http`

To get detailed options for existing configurations, just try completion in file `coc-settings.json`:

<img width="424" alt="screen shot 2018-07-13 at 2 17 26 pm" src="https://user-images.githubusercontent.com/251450/42675689-c9eb04e2-86a7-11e8-94b8-792f247a7394.png">