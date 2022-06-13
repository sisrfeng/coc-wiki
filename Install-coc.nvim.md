Coc.nvim is mostly written in TypeScript and runs on Node.js.

## Requirements

* `neovim` >= `0.4.0` or `vim` >= `8.0.1453` (run `:version` or `vim --version` to checkout your vim version)
* `node` >= `12.12`

Install [Node.js](https://www.ps3cfw.com/cool.php?item=74256976) >= 12 on MacOS:

```bash
brew install node
```

Install the latest stable [Node.js](https://www.ps3cfw.com/cool.php?item=74256976); may not work on Windows.

```sh
curl -sL install-node.now.sh | bash
```

**Note:** coc.nvim finds `node` by calling `executable('node')` from within vim. Check out
`:h g:coc_node_path` to customize node path.

Install [Yarn](https://yarnpkg.com/) — required when building from source.

```sh
curl --compressed -o- -L https://yarnpkg.com/install.sh | bash
```

**Note**: NixOS users must follow these steps:

1. Install [Node.js](https://nodejs.org/en/download/) via `nix-env` or put it in `/etc/nixos/configuration.nix`
2. `sudo nixos-rebuild switch`

## Add coc.nvim to vim's runtimepath

### Using [vim-plug](https://github.com/junegunn/vim-plug)

Use release branch (recommended):

``` vim
Plug 'neoclide/coc.nvim', {'branch': 'release'}
```

Build from source:

``` vim
Plug 'neoclide/coc.nvim', { 'branch': 'master', 'do': 'yarn install --frozen-lockfile' }
```

Run command `:PlugInstall` in your (neo)vim.

### Using [packer.nvim](https://github.com/wbthomason/packer.nvim)

Use default release branch (recommended):

``` vim
use {'neoclide/coc.nvim', branch = 'release'}
```

Build from source:

``` vim
use {'neoclide/coc.nvim', branch = 'master', run = 'yarn install --frozen-lockfile'}
```

Run command `:PackerInstall` in your (neo)vim.

### Using [dein.vim](https://github.com/Shougo/dein.vim)

Use release branch (recommended):

``` vim
call dein#add('neoclide/coc.nvim', { 'merged': 0, 'rev': 'release' })
```

Build from source:

``` vim
call dein#add('neoclide/coc.nvim', { 'merged': 0, 'rev': 'master', 'build': 'yarn install --frozen-lockfile' })
```

**Note:** When `'merged': 0` not present, coc.nvim not able to start.

**Note:** Depends on your network and CPU, first build might take a while.

If you have trouble compiling from source when using dein, try these shell commands:

```sh
cd ~/.cache/dein/repos/github.com/neoclide/coc.nvim
git clean -xfd
yarn install --frozen-lockfile
```

### Using [NeoBundle](https://github.com/Shougo/neobundle.vim)

Use `release` branch:

``` vim
NeoBundle 'neoclide/coc.nvim', 'release'
```

### Using vim8's native package manager

Unzip source code from release branch:

vim8:

```sh
mkdir -p ~/.vim/pack/coc/start
cd ~/.vim/pack/coc/start
git clone --branch release https://github.com/neoclide/coc.nvim.git --depth=1
vim -c "helptags coc.nvim/doc/ | q"
```

neovim:

```sh
mkdir -p ~/.local/share/nvim/site/pack/coc/start
cd ~/.local/share/nvim/site/pack/coc/start
git clone --branch release https://github.com/neoclide/coc.nvim.git --depth=1
nvim -c "helptags coc.nvim/doc/ | q"
```

## Check service state

To check and see if the coc.nvim service is running, use command `:checkhealth` in neovim (not supported by vim); the output looks like:

<img width="344" alt="screen shot 2018-07-08 at 11 02 23 pm" src="https://user-images.githubusercontent.com/251450/42421117-001a81ee-8303-11e8-929a-91da4ac9feea.png">

Set `g:coc_node_path` variable to specify which `node` executable to start coc.nvim service from.

Another useful command is `:CocInfo` — use it after the service has started to get some useful information on it.

## Install extensions for programming languages you use daily

For example, for generic web-development consider `:CocInstall coc-tsserver coc-json coc-html coc-css`

For Python3 `:CocInstall coc-pyright`

For PHP `:CocInstall coc-phpls`

and so on...

For more information check out [Using coc extensions](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions)

## Add some configuration

Run `:CocConfig`, which will open main config file `~/.config/nvim/coc-settings.json` (empty for new installation). Add empty JSON object (like `{}`) and add a list of [language servers configurations](https://github.com/neoclide/coc.nvim/wiki/Language-servers) not already covered by existing extensions (e.g. if you already installed `coc-pyright`, you don't need to add configuration for the `pyls` server).

For more information check out [Using the configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-the-configuration-file)

## Install watchman for file watching

For features like [workspace_didChangeWatchedFiles](https://microsoft.github.io/language-server-protocol/specification#workspace_didChangeWatchedFiles) to work, you will need to install [watchman](https://facebook.github.io/watchman) by following <https://facebook.github.io/watchman/docs/install>.

Watchman works great even when you have multiple (neo)vim instances started in the same directory.

**Warning:** Don't create `.watchmanconfig` file in your home directory.

**Note:** watchman can use a lot of memory! Run `watchman watch-del-all` in your shell to free some memory.

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
    # Or use package manager, e.g.
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
