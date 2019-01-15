Coc is written is Typescript and runs in nodejs, you can download pre build binary file or build from source.

## Install [neovim](https://github.com/neovim/neovim/releases/) or [vim](https://github.com/vim/vim) 

Neovim >= 0.3.0 is required. 

vim >= 0.8.1 is supported.

Use `:version` to checkout your vim version.

## Install [nodejs](https://nodejs.org/) >= 8.0

```
curl -sL install-node.now.sh | sh
```

## Install [yarn](https://yarnpkg.com/)

If you want to build source code or install extension, yarn is requried.

```
curl --compressed -o- -L https://yarnpkg.com/install.sh | bash
```

## Install plugin coc.nvim

Take [junegunn/vim-plug](https://github.com/junegunn/vim-plug) for example, add

``` vim
Plug 'neoclide/coc.nvim', {'do': { -> coc#util#install()}}
```

To build from source, add:

``` vim
Plug 'neoclide/coc.nvim', {'do': 'yarn install'}
```
to your `.vimrc` and run command `:PlugInstall` in your neovim.

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

# Install vim-node-rpc for vim
# yarn global add -g vim-node-rpc

# Change the path to where coc installed
cd ~/.vim/bundle/coc.nvim
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