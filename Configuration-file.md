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
``` 

