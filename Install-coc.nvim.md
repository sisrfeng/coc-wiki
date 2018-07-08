Coc is a remote plugin of [neovim](https://github.com/neovim/neovim) which requires additional setup to make it work.

## Install [nodejs](https://nodejs.org/)

Official download page: https://nodejs.org/en/download/package-manager/, for Mac user, [homebrew](https://brew.sh/) is recommended

```
brew install node
```

For coc to work, the version of nodejs should >= 8.0, use command:

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

It's a node module required for communication between neovim and node remote plugins.

Use command `npm install -g neovim` or `yarn add -g neovim` to install it.

neovim could take almost 200ms to find out location of neovim node module, you can make your neovim faster by tell neovim where the module is.

locate the global node module location by using command:

```
npm --loglevel silent root -g
```

retsulte should be something like `/usr/local/lib/node_modules`, and then add:

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
