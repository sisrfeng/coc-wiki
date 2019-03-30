Coc is written is Typescript and runs in nodejs, you can download pre build binary file or build from source.

## Install [neovim](https://github.com/neovim/neovim/releases/) or [vim](https://github.com/vim/vim) 

* `neovim` >= `0.3.1` is required.
* `vim` >= `8.1` is required.
* Use command `:version` to checkout your vim version.

**Note:** it will not load at all when version not match.

## Install [nodejs](https://nodejs.org/) >= 8.0

```
curl -sL install-node.now.sh | sh
```

## Install [yarn](https://yarnpkg.com/)

Yarn is required for build coc.nvim from source code and manage extensions.

```
curl --compressed -o- -L https://yarnpkg.com/install.sh | bash
```

## Install plugin coc.nvim

### Using [vim-plug](https://github.com/junegunn/vim-plug)

Install binary release:

``` vim
Plug 'neoclide/coc.nvim', {'tag': '*', 'do': { -> coc#util#install()}}
```

If you're using binary release `'tag': '*'` is recommended, it helps you upgrade coc.nvim only when new release available.

Build from source code:

``` vim
Plug 'neoclide/coc.nvim', {'do': 'yarn install --frozen-lockfile'}
```
to your `.vimrc` and run command `:PlugInstall` in your neovim.

## Using [dein.vim](https://github.com/Shougo/dein.vim)

It's recommended to build coc.nvim from source code since dein can't upgrade with new release only.

Add:
```
call dein#add('neoclide/coc.nvim', {'merge':0, 'build': 'yarn install --frozen-lockfile'})
```
**Note:** when `'merge':0` not present, coc.nvim would be unable to start. 

**Note:** depends on your network and CPU, it might take a long time for the first time build. 

## Checkout service state.

To check out coc service is running, use command `:checkhealth` in neovim (not supported by vim), the output should include:

<img width="344" alt="screen shot 2018-07-08 at 11 02 23 pm" src="https://user-images.githubusercontent.com/251450/42421117-001a81ee-8303-11e8-929a-91da4ac9feea.png">



## Optional install watchman for file watching

For feature [workspace_didChangeWatchedFiles](https://microsoft.github.io/language-server-protocol/specification#workspace_didChangeWatchedFiles) to work, you will need to install [watchman](https://facebook.github.io/watchman) by following https://facebook.github.io/watchman/docs/install.html.

Watchman works great even when you have multiple neovim instance started in the same directory.

**Warning** don't create `.watchmanconfig` file in your home directory.

## Automation script

For setup coc and extensions faster on different machines, you can use shell script, for example:

``` sh
#!/bin/sh

set -o nounset    # error when referencing undefined variable
set -o errexit    # exit when command fails

# Install latest nodejs
if ! command -v node > /dev/null; then
  curl --fail -L https://install-node.now.sh/latest | sh
  # Or use apt-get
  # sudo apt-get install nodejs
fi

# Install yarn
if ! command -v yarn > /dev/null; then
  curl --fail -L https://yarnpkg.com/install.sh | sh
fi

# vim-node-rpc is required for vim only
# yarn global add -g vim-node-rpc

# Use package feature to install coc.nvim
# If you want to use plugin manager, change DIR to plugin directory used by that manager.
DIR=~/.local/share/nvim/site/pack/coc/start
# For vim user, the directory is different
# DIR=~/.vim/pack/coc/start
mkdir -p $DIR
cd $DIR
git clone https://github.com/neoclide/coc.nvim.git --depth=1
cd $DIR/coc.nvim
yarn install

# Install extensions
mkdir -p ~/.config/coc/extensions
cd ~/.config/coc/extensions
if [ ! -f package.json ]
then
  echo '{"dependencies":{}}'> package.json
fi
# Change arguments to extensions you need
yarn add coc-json coc-snippets
```