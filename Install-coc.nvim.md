Coc is a remote plugin of [neovim](https://github.com/neovim/neovim) which requires additional setup to make it work.

## Install [neovim](https://github.com/neovim/neovim/releases/)

Neovim > 0.3.0 is recommended. 

The completion resolve feature and buffer increment sync feature would not be available if neovim < 0.3.0

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

## Install [node-client](https://github.com/neovim/node-client)

```
npm install -g neovim
```
**Note:** sudo might be required.

It's a node module required for communication between neovim and node remote plugins.

neovim could take almost 200ms to find out location of neovim node module, you can make your neovim faster by tell neovim where the module is.

locate the global node module location by using command:

```
npm --loglevel silent root -g
```

result should be something like `/usr/local/lib/node_modules`, and then add:

``` vim
let g:node_host_prog = '/usr/local/lib/node_modules/neovim/bin/cli.js'
```
to your `.vimrc`.

## Install plugin coc.nvim

Take [junegunn/vim-plug](https://github.com/junegunn/vim-plug) for example, add

``` vim
Plug 'neoclide/coc.nvim', {'do': 'yarn install'}
```

to your `.vimrc` and run command `:PlugInstall` in your neovim.

Finally, run command `:UpdateRemotePlugins` in neovim after plugin installed.

When coc is successfully registered, the output should looks like:

<img width="442" alt="screen shot 2018-07-08 at 10 46 12 pm" src="https://user-images.githubusercontent.com/251450/42421029-43c838f2-8301-11e8-88af-19203a5eca91.png">

To check out coc service is running, use command `:checkhealth` in neovim, the output should looks like:

<img width="344" alt="screen shot 2018-07-08 at 11 02 23 pm" src="https://user-images.githubusercontent.com/251450/42421117-001a81ee-8303-11e8-929a-91da4ac9feea.png">

## Optional install watchman for file watching

For feature [workspace_didChangeWatchedFiles](https://microsoft.github.io/language-server-protocol/specification#workspace_didChangeWatchedFiles) to work, you will need to install [watchman](https://facebook.github.io/watchman) by following https://facebook.github.io/watchman/docs/install.html.

Watchman works great even when you have multiply neovim instance started in the same directory.

The `UpdatePathOnRename` feature provided by tsserver requires file watching to work.

