Coc.nvim is written in Typescript and runs on nodejs, you can use release branch which contains compiled javascript or build from master branch.

## Install [neovim](https://github.com/neovim/neovim/releases/) or [vim](https://github.com/vim/vim) 

* `neovim` >= `0.3.1` is required.
* `vim` >= `8.0.1453` is required.
* Use command `:version` to checkout your vim version.

**Note:** it will not load at all if (neo)vim is too old.

Install [nodejs](https://nodejs.org/) >= 8.10.0 on MacOS:

```bash
brew install node
```

Install the latest stable [nodejs](https://nodejs.org/), may not work on windows.

```
curl -sL install-node.now.sh | sh
```

Install [yarn](https://yarnpkg.com/)

Yarn is required for build coc.nvim from source code and manage extensions.

```
curl --compressed -o- -L https://yarnpkg.com/install.sh | bash
```

**Note** yarn is not required if you want to use vim's plugin manager to manage coc extensions.

## Add coc.nvim to your vim's runtimepath.

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
call dein#add('neoclide/coc.nvim', {'merge':0, 'rev': 'release'})
```

Build from source code:

``` vim
call dein#add('neoclide/coc.nvim', {'merge':0, 'build': 'yarn install --frozen-lockfile'})
```

**Note:** when `'merge':0` not present, coc.nvim will be unable to start. 

**Note:** depends on your network and CPU, it might take a long time for your first build. 

If your have trouble with compiling the source code when using dein, try command:

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

## Checkout service state.

To check to see if the coc service is running, use command `:checkhealth` in neovim (not supported by vim), the output looks like:

<img width="344" alt="screen shot 2018-07-08 at 11 02 23 pm" src="https://user-images.githubusercontent.com/251450/42421117-001a81ee-8303-11e8-929a-91da4ac9feea.png">

Use `g:coc_node_path` variable to specify node executable that start service of coc.nvim.

Another useful command is `:CocInfo`, after service started, you can use it get some useful information about the service.

## Optional install watchman for file watching

For feature [workspace_didChangeWatchedFiles](https://microsoft.github.io/language-server-protocol/specification#workspace_didChangeWatchedFiles) to work, you will need to install [watchman](https://facebook.github.io/watchman) by following https://facebook.github.io/watchman/docs/install.html.

Watchman works great even when you have multiple neovim instance started in the same directory.

**Warning** don't create `.watchmanconfig` file in your home directory.

## Automation script

To setup coc and extensions faster on different machines, you can use a shell script, for example:

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

# Install yarn
if [ ! -x "$(command -v yarn)" ]; then
    curl --fail -o- -LSs https://yarnpkg.com/install.sh | sh
    export PATH="$HOME/.yarn/bin:$HOME/.config/yarn/global/node_modules/.bin:$PATH"
fi

# Use package feature to install coc.nvim

# for vim8
mkdir -p ~/.vim/pack/coc/start
cd ~/.vim/pack/coc/start
curl --fail -L https://github.com/neoclide/coc.nvim/archive/release.tar.gz|tar xzfv -
# for neovim
# mkdir -p ~/.local/share/nvim/site/pack/coc/start
# cd ~/.local/share/nvim/site/pack/coc/start
# curl --fail -L https://github.com/neoclide/coc.nvim/archive/release.tar.gz|tar xzfv -

# Install extensions
mkdir -p ~/.config/coc/extensions
cd ~/.config/coc/extensions
if [ ! -f package.json ]
then
  echo '{"dependencies":{}}'> package.json
fi
# Change arguments to the extensions you need
yarn add coc-snippets coc-highlight
```