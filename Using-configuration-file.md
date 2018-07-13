COC.nvim support different scope of configuration files in JSON5 format.
# Contents

* [Configuration file format](#configuration-file-format)
* [Configuration file resolve](#configuration-file-resolve)
* [Default COC preferences](#default-coc-preferences)
* [Configuration for sources](#configuration-for-sources)
* [Tsserver configuration](#tsserver-configuration)
* [Typescript/javascript configuration](#typescriptjavascript-configuration)

## Configuration file format

COC.nvim use [jsonc](https://code.visualstudio.com/docs/languages/json) as configurations file format, the same as VSCode.
It's basically json with comment, like:

``` json
{
  // my variable
  "foo": "bar"
}
```

You can make use of [jsonc.vim](https://github.com/chemzqm/jsonc.vim) to get correct highlight for comment.

## Configuration file resolve

There're three types of configuration file for COC.

* The default one bundles with this plugin, which is [settings.json](https://github.com/neoclide/coc.nvim/blob/master/settings.json)

* The user configuration should be named as `coc-settings.json` and placed inside folder `$VIMCONFIG` or `$XDG_CONFIG_HOME/nvim` or `$HOME/.config/nvim`, for Linux/MaxOS user, the file could be created by

        echo -e '{\n  "$schema": "https://raw.githubusercontent.com/neoclide/coc.nvim/master/data/schema.json"\n}'\
        > ~/.config/nvim/coc-settings.json

    '$schema' is used for provide intellisense for json server.

* The project configuration should be named with `coc-settings.json` and would be resolve in directory `.vim`. Just after vim started, COC would look up from current directory of vim to find it.

The active configuration would be a merged result from 'default', 'user' and 'project' configuration file, **the later one have higher priority**.

If your want to know all exists settings, check out[settings.json](https://github.com/neoclide/coc.nvim/blob/master/settings.json).

## Default COC preferences

``` js
  // could be 'always' 'trigger' => for specified trigger characters only, 'none' => for disable auto trigger
  "coc.preferences.autoTrigger": "always",
  // timeout for completion
  "coc.preferences.timeout": 300,
  // not make vim select first item on completion start
  "coc.preferences.noselect": true,
  // executable path for https://facebook.github.io/watchman/, detected from $PATH by default
  "coc.preferences.watchmanPath": "",
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

## Tsserver configuration

Tsserver configuration used for customize how tsserver works.

``` js
  // language locale
  "tsserver.locale": null,
  // folder of tsserver.js
  // Default priority: module in node_modules > module in $PATH > bundled module
  "tsserver.tsdk": "",
  // absolute path of npm, infered from $PAHT if empty
  "tsserver.npm": "",
  // could be 'normal' 'terse' 'verbose' 'off'
  "tsserver.log": "off",
  // could be 'off' 'messages' 'verbose', could also be provided by environment variable 'TSS_TRACE'
  "tsserver.trace": "off",
  // module name of global plugins
  "tserver.pluginNames": [],
  // folder root of global plugins
  "tsserver.pluginRoot": "",
  // could also be provided by environment variable `TSS_DEBUG`
  "tsserver.debugPort": null,
  "tsserver.reportStyleChecksAsWarnings": true,
  "tsserver.implicitProjectConfig.checkJs": false,
  "tsserver.implicitProjectConfig.experimentalDecorators": false,
  "tsserver.disableAutomaticTypeAcquisition": false,
  // enable support for javascript file
  "tsserver.enableJavascript": false,
``` 

## Typescript/javascript configuration

The 'typescript' and 'javascript' configurations contains information about file format and complete option, they are used by extension `tsserver`, check out [settings/default.json](https://github.com/neoclide/coc.nvim/blob/master/settings/default.json) for available options.

