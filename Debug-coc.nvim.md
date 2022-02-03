This article contains tips for debug coc.nvim, if you have issues with a specific language server,
checkout https://github.com/neoclide/coc.nvim/wiki/Debug-language-server first.

## Build source code

First, install [yarn](https://yarnpkg.com/) to your global `$PATH`

After cloning the repo, make sure you're using master branch of coc.nvim, then install dependencies
and build source code by:

```
yarn install
```

The javascript bundle will exists in `build` folder.

To build source code after changes made, run:

```
node esbuild.js --watch
``` 
in project root, which will use esbuild to compile source code to single javascript bundle.

After each compile, restart coc.nvim service by `:CocRestart` to use new javascript code.

Restart your vim is needed after you've made changes to vim code of coc.nvim.

## Enable source map support

To create source map of javascript bundle, use:

```
NODE_ENV=development node esbuild.js
```

To compile source code.

To enable source map for error stack, install [source-map-support](https://github.com/evanw/node-source-map-support) by:

```
yarn global add source-map-support
```
Then use configuration like:
``` vim
" use command `yarn global dir` in your terminal to checkout yarn global directory.
let g:coc_node_args = ['-r', expand('~/.config/yarn/global/node_modules/source-map-support/register')]
```
in your vimrc, it will make trace error much easier.

## Get result from console

**Warning:** you should avoid usage of `process.stdout`, `process.stdin` and related method from `console`, since coc.nvim has to use stdio for communication between (neo)vim.

You can use `console.error` to write a string message, like:

``` js
console.error('my error')
```
The message will be echoed in vim. However, this method is quite limited.

## Use logger module

Use command `:CocOpenLog` to open the log file.

Import the logger module for log purpose, like:

``` js
const logger = require('./util/logger')('workspace')
```
Use the logger to debug any variable, like:

``` js
logger.debug('variable:', variable)
```

For extension of coc.nvim, usea `logger` object (`log4js.Logger`) through a property of `ExtensionContext`. For example:

```js
exports.activate = async (context) => {
  let { logger } = context;
  logger.info(`Extension from ${context.extensionPath}`)
}
```

If you're using `console.log` in extension, the output will append to the log of coc.nvim.

The default log level is `info`, so you won't get `debug` or `trace` messages shown in `:CocOpenLog`.
To change the log level, you need to configure environment variable `NVIM_COC_LOG_LEVEL`, check out
`:h :CocOpenLog` for details.

## Inspect communication between vim and coc.nvim

Enable the client log by:
``` vim
let g:node_client_debug = 1
let $NODE_CLIENT_LOG_FILE = '/path/to/logfile'
```
In your vimrc, then open the `$NODE_CLIENT_LOG_FILE` in another terminal or use `:call coc#client#open_log()`
to open the log use current vim session.


## Use debugger of nodejs

Add:
```
  let g:coc_node_args = ['--nolazy', '--inspect=6045']
```
to your `.vimrc`

After restart coc, you will get error message like:
```
[vim-node-coc]: Debugger listening on ws://127.0.0.1:6045/cd7eea09-c79f-4100-b4a0-bfbb43e94f48
For help, see: https://nodejs.org/en/docs/inspector
```
it means the debugger protocol is started.

Open url `chrome://inspect` in chrome, make sure `Discover network targets` is checked and then click `configure...` button:

<img width="363" alt="screen shot 2018-12-24 at 10 13 40 am" src="https://user-images.githubusercontent.com/251450/50389401-d1d48280-0764-11e9-941e-c7faa92b8603.png">

then add `127.0.0.1:6045` to `Target discovery settings`.

Checkout remote target section, and then click `inspect` for the nodejs target:

<img width="345" alt="screen shot 2018-12-24 at 10 15 52 am" src="https://user-images.githubusercontent.com/251450/50389417-12340080-0765-11e9-85ac-f1529e6d79b9.png">

For more details of debugging, checkout https://nodejs.org/en/docs/guides/debugging-getting-started.