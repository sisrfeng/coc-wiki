# Configuration file format

COC.nvim use [json5](https://github.com/json5/json5) as configurations file format, like VSCode.
It's basically json with comment, like:

``` json
{
  // my variable
  "foo": "bar"
}
```

You can make use of [json5.vim](https://github.com/gutenye/json5.vim) to get correct highlight for comment.

# Configuration file resolve

There're three types of configuration file for COC.

* The default one comes with this plugin, located at [settings/default.json](https://github.com/neoclide/coc.nvim/blob/master/settings/default.json)

* The user configuration should be named with `coc-settings.json` and placed inside folder `$VIMCONFIG` or `$XDG_CONFIG_HOME/nvim` or `$HOME/.vim`, it's recommended create one for yourself, but not required.

* The project configuration should be named with `coc-settings.json` and would be resolve in directory `.vim`. Just after vim started, COC would look up from current directory of vim to find it.

The active configuration would be a merged result from 'default', 'user' and 'project' configuration file, the later one have higher priority.

You don't have to create a configuration file from sketch, copy [settings/default.json](https://github.com/neoclide/coc.nvim/blob/master/settings/default.json) to get start.

## Configuration for sources

Common sources and vim sources are configured with prefix `coc.source.{sourcenam}` in configuration file, they share some common attributes, take 'neco' source for example:

``` js
  // Set to true to disable a source totally
  "coc.source.neco.disabled": false,
  // Short words used for menu of complete item
  "coc.source.neco.shortcut": "NEC",
  // priority of this source
  "coc.source.neco.priority": 9,
  // filetypes using this source, set empty means disable source, if not defined, it works for all filetypes
  "coc.source.neco.filetypes": ["vim"],
  // only show the complete item that first letter match the input, works for vim source only.
  "coc.source.neco.firstMatch": true,
  // List of none-word characters for trigger completion
  "coc.source.file.triggerCharacters": [],
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

