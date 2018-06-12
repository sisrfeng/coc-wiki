
# Configuration file

COC.nvim could use some [json5](https://github.com/json5/json5) formated file as configurations.

There're three types of configuration file for COC.

* The default one comes with this plugin, at [settings/default.json](https://github.com/neoclide/coc.nvim/blob/master/settings/default.json)

* The user configuration should be named with `coc-settings.json` and placed inside folder `$VIMCONFIG` or `$XDG_CONFIG_HOME/nvim` or `$HOME/.vim`

* The project configuration should be named with `coc-settings.json` and would be resolve in directory `.vim`, COC would look up from current directory to find it.

The active configuration would be a merged result from 'default', 'user' and 'project' configuration file, the later one have higher priority.

You don't have to create a configuration file from sketch, copy the file [settings/default.json](https://github.com/neoclide/coc.nvim/blob/master/settings/default.json) to get start.

