COC.nvim use [jsonc](https://code.visualstudio.com/docs/languages/json) as configurations file format, the same as VSCode.
It's basically json with comment, like:

``` json
{
  // my variable
  "foo": "bar"
}
```

You can make use of [jsonc.vim](https://github.com/chemzqm/jsonc.vim) to get correct highlight for comment.

# Contents

* [Configuration file resolve](#configuration-file-resolve)
* [Default COC preferences](#default-coc-preferences)
* [Configuration for sources](#configuration-for-sources)
* [Extension configuration](#extension-configuration)

## Configuration file resolve

There're three types of configuration file for COC.

* The default one bundles with this plugin, which is [settings.json](https://github.com/neoclide/coc.nvim/blob/master/settings.json)

* The user configuration should be named as `coc-settings.json` and placed inside folder `$VIMCONFIG` or `$XDG_CONFIG_HOME/nvim` or `$HOME/.config/nvim`, for Linux/MaxOS user, the file could be created by

        echo -e '{}' > ~/.config/nvim/coc-settings.json

* The workspace configuration should be named with `coc-settings.json` and would be resolve in directory `.vim`. Just after vim started, coc would look up from current directory of vim to find it.

The active configuration would be a merged result from 'default', 'user' and 'workspace' configuration file, **the later one have higher priority**.

The intellisense of file `coc-settings.json` is enabled automatically. If your want to know default settings, check out [settings.json](https://github.com/neoclide/coc.nvim/blob/master/settings.json).

## Default COC preferences

``` js
  // could be 'always' 'trigger' => for specified trigger characters only 'none'
  "coc.preferences.autoTrigger": "always",
  // timeout for completion
  "coc.preferences.timeout": 300,
  // not make vim select first item on completion start
  "coc.preferences.noselect": true,
  // executable path for https://facebook.github.io/watchman/, detected from $PATH by default
  "coc.preferences.watchmanPath": "",
  // enable diagnostic
  "coc.preferences.diagnostic.enable": true,
  "coc.preferences.diagnostic.signOffset": 1000,
  "coc.preferences.diagnostic.errorSign": ">>",
  "coc.preferences.diagnostic.warningSign": "⚠",
  "coc.preferences.diagnostic.infoSign": ">>",
  "coc.preferences.diagnostic.hintSign": ">>",
``` 

**Note:** for configure keymappings of coc, vim global variable is used to make is easier to work with, check out `coc-variable` section at `doc/coc.txt`.

## Configuration for sources

Sources are configured with prefix `coc.source.{sourcename}` in configuration file, they share some common attributes, take 'neco' source for example:

``` js
  // Set to false to disable a source totally
  "coc.source.neco.enable": true,
  // Short words used for menu of complete item
  "coc.source.neco.shortcut": "NEC",
  // priority of this source
  "coc.source.neco.priority": 9,
  // filetypes using this source, set empty means disable source, if not defined, it works for all filetypes
  "coc.source.neco.filetypes": ["vim"],
  // only show the complete item that first letter match the input, works for vim source only.
  "coc.source.neco.firstMatch": true,
  // List of none-word characters for trigger completion
  "coc.source.neco.triggerCharacters": [":"],
```

## Extension configuration

* `tsserver` configuration used for customize how tsserver works.
* `typescript` and `javascript` configurations contains information about file format and complete option which are used by tsserver.
* `csserver` is used by css server extension.
* `http` and `json` is used by json server extension.
* `html` is used by html server extension.
* `wxml` is used by wxml server extension.
* `tslint` is used by tslint server extension.
* `eslint` is used by eslint server extension.
* `stylelint` is used by stylelint server extension. 
* `solargraph` is used by solargraph extension.
* `vetur` is used by vetur extension.

To get detailed options for configuration, just try completion in file `coc-settings.json`

<img width="424" alt="screen shot 2018-07-13 at 2 17 26 pm" src="https://user-images.githubusercontent.com/251450/42675689-c9eb04e2-86a7-11e8-94b8-792f247a7394.png">

