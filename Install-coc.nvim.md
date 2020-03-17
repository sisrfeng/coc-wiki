Coc.nvim is mostly written in typescript and runs on nodejs.

You can use release branch which contains compiled javascript (build/index.js) or build from master branch.

## Requirements

* `neovim` >= `0.3.1` or `vim` >= `8.0.1453` (use command `:version` to checkout your vim version.)
* `node` >= `8.10.0`

**Note:** it will not load at all if (neo)vim is too old.

Install [nodejs](https://nodejs.org/) >= 10 on MacOS:

```bash
brew install node
```

Install the latest stable [nodejs](https://nodejs.org/), may not work on windows.

```sh
curl -sL install-node.now.sh | sh
```

**Note:** coc.nvim find `node` by function `executable('node')` in your vim, checkout
`:h g:coc_node_path` for customize node path.  

Install [yarn](https://yarnpkg.com/) needed when you want to build from source code.

```sh
curl --compressed -o- -L https://yarnpkg.com/install.sh | bash
```

**Note**: NixOS users must follow these steps:

1. Install [nodejs](https://nodejs.org/en/download/) via `nix-env` or put them in `/etc/nixos/configuration.nix`
2. `sudo nixos-rebuild switch`

## Add coc.nvim to your vim's runtimepath

### Using [vim-plug](https://github.com/junegunn/vim-plug)

Use release branch (recommended):

``` vim
Plug 'neoclide/coc.nvim', {'branch': 'release'}
```

Build from source code:

``` vim
Plug 'neoclide/coc.nvim', {'do': 'yarn install --frozen-lockfile'}
```

Run command `:PlugInstall` in your (neo)vim.

### Using [dein.vim](https://github.com/Shougo/dein.vim)

Use release branch (recommended):

``` vim
call dein#add('neoclide/coc.nvim', {'merged':0, 'rev': 'release'})
```

Build from source code:

``` vim
call dein#add('neoclide/coc.nvim', {'merged':0, 'build': 'yarn install --frozen-lockfile'})
```

**Note:** when `'merged':0` not present, coc.nvim will be unable to start.

**Note:** depends on your network and CPU, it might take a long time for your first build.

If you have trouble with compiling the source code when using dein, try command:

``` sh
cd ~/.cache/dein/repos/github.com/neoclide/coc.nvim
git clean -xfd
yarn install --frozen-lockfile
```

### Using [NeoBundle](https://github.com/Shougo/neobundle.vim)

Due to [this bug](https://github.com/Shougo/neobundle.vim/issues/530), using the standard `'rev': 'release'` won't work.

Use this work-around to check out the recommended `release` branch:

``` vim
NeoBundle 'neoclide/coc.nvim', 'release', { 'build': { 'others': 'git checkout release' } }
```

### Using vim8's native package manager

Unzip source code from release branch:

```sh
# for vim8
mkdir -p ~/.vim/pack/coc/start
cd ~/.vim/pack/coc/start
curl --fail -L https://github.com/neoclide/coc.nvim/archive/release.tar.gz|tar xzfv -
# for neovim
mkdir -p ~/.local/share/nvim/site/pack/coc/start
cd ~/.local/share/nvim/site/pack/coc/start
curl --fail -L https://github.com/neoclide/coc.nvim/archive/release.tar.gz|tar xzfv -
```

## Checkout service state

To check to see if the coc service is running, use command `:checkhealth` in neovim (not supported by vim), the output looks like:

<img width="344" alt="screen shot 2018-07-08 at 11 02 23 pm" src="https://user-images.githubusercontent.com/251450/42421117-001a81ee-8303-11e8-929a-91da4ac9feea.png">

Use `g:coc_node_path` variable to specify node executable that start service of coc.nvim.

Another useful command is `:CocInfo`, after service started, you can use it get some useful information about the service.

## Install extension for programming languages you use daily

For example, for generic web-development consider `:CocInstall coc-tsserver coc-json coc-html coc-css`

For Python `:CocInstall coc-python`

For PHP `:CocInstall coc-phpls`

and so on...

For more information check out [Using coc extensions](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions)

## Add some configuration

Run `:CocConfig`, which will open main config file `~/.config/nvim/coc-settings.json` (empty for new installation). Add empty JSON object (like `{}`) and place here list of [language servers configuration](https://github.com/neoclide/coc.nvim/wiki/Language-servers) that not is not covered by installed extensions (for example, if you already installed `coc-python` you don't need to place configuration for `pyls` server).

For more information check out [Using the configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-the-configuration-file)

## Optional install watchman for file watching

For feature [workspace_didChangeWatchedFiles](https://microsoft.github.io/language-server-protocol/specification#workspace_didChangeWatchedFiles) to work, you will need to install [watchman](https://facebook.github.io/watchman) by following <https://facebook.github.io/watchman/docs/install.html>.

Watchman works great even when you have multiple (neo)vim instance started in the same directory.

**Warning** don't create `.watchmanconfig` file in your home directory.

**Note** watchman can take lots of memories, use command `watchman watch-del-all` in your terminal to free your memory.

## Automation script

To set up coc.nvim and extensions faster on different machines, you can use a shell script, for example:

``` sh
#!/usr/bin/env bash

set -o nounset    # error when referencing undefined variable
set -o errexit    # exit when command fails

# Install latest nodejs
if [ ! -x "$(command -v node)" ]; then
    curl --fail -LSs https://install-node.now.sh/latest | sh
    export PATH="/usr/local/bin/:$PATH"
    # Or use apt-get
    # sudo apt-get install nodejs
fi

# Use package feature to install coc.nvim

# for vim8
mkdir -p ~/.vim/pack/coc/start
cd ~/.vim/pack/coc/start
curl --fail -L https://github.com/neoclide/coc.nvim/archive/release.tar.gz | tar xzfv -
# for neovim
# mkdir -p ~/.local/share/nvim/site/pack/coc/start
# cd ~/.local/share/nvim/site/pack/coc/start
# curl --fail -L https://github.com/neoclide/coc.nvim/archive/release.tar.gz | tar xzfv -

# Install extensions
mkdir -p ~/.config/coc/extensions
cd ~/.config/coc/extensions
if [ ! -f package.json ]
then
  echo '{"dependencies":{}}'> package.json
fi
# Change extension names to the extensions you need
npm install coc-snippets --global-style --ignore-scripts --no-bin-links --no-package-lock --only=prod
```
