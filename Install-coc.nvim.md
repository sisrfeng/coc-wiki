Coc is written is Typescript and runs in nodejs, code compile is required to make it work.

## Install [neovim](https://github.com/neovim/neovim/releases/) or [vim](https://github.com/vim/vim) 

Neovim >= 0.3.0 is required. 

vim >= 0.8.1 is supported.

Use `:version` to checkout your vim version.

## Install [nodejs](https://nodejs.org/)

Official download page: https://nodejs.org/en/download/package-manager/, for Mac user, [homebrew](https://brew.sh/) is recommended

```
brew install node
```

Nodejs version should >= 8.0, use command:

```
node -v
```
to check out.

## Install [yarn](https://yarnpkg.com/)

Official download page: https://yarnpkg.com/en/docs/install, for Mac user, [homebrew](https://brew.sh/) is recommended

```
brew install yarn
```

To make sure yarn available in your `$PATH`, use
```
yarn -h
```

**Note** Global [node-client](https://github.com/neovim/node-client) not required any more

## Install plugin coc.nvim

Take [junegunn/vim-plug](https://github.com/junegunn/vim-plug) for example, add

``` vim
Plug 'neoclide/coc.nvim', {'do': 'yarn install'}
```

to your `.vimrc` and run command `:PlugInstall` in your neovim.

**Note** `:UpdateRemotePlugins` not required any more.

To check out coc service is running, use command `:checkhealth` in neovim, the output should include:

<img width="344" alt="screen shot 2018-07-08 at 11 02 23 pm" src="https://user-images.githubusercontent.com/251450/42421117-001a81ee-8303-11e8-929a-91da4ac9feea.png">

## Optional install watchman for file watching

For feature [workspace_didChangeWatchedFiles](https://microsoft.github.io/language-server-protocol/specification#workspace_didChangeWatchedFiles) to work, you will need to install [watchman](https://facebook.github.io/watchman) by following https://facebook.github.io/watchman/docs/install.html.

Watchman works great even when you have multiply neovim instance started in the same directory.

The `UpdatePathOnRename` feature provided by tsserver requires file watching to work.

