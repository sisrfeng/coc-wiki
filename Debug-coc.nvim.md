Coc.nvim is mostly written in typescript, which compiles to javascript. If you know javascript or how nodejs works, it will be easier to debug the source code.

## Commands to build source code

After cloning the repo, you need build the source code first, this can be done by running:

```
yarn install
```

it can be slow the first time, since some dependencies need to be downloaded. This command will also invoke `yarn clean` and `yarn build` commands.  `yarn clean` will remove the build directory which contains the binary release. `yarn build` will build your source code.

To build source code after making changes, run:

```
yarn build
``` 

which uses tsserver to compile typescript to javascript.

However, running `yarn build` takes some time. To get faster incremental builds, you need to run:

```
yarn watch
```
which enables the watch option for tsserver. It will be much faster for rebuilding the source code.

After each compile, restart coc service by `:CocRestart` to make new server take control.

**TIP** you can use watch build feature provided by `coc-tsserver` like:

``` vim
command! -nargs=0 Tsc :call CocAction('runCommand', 'tsserver.watchBuild')
```
With statusline integration, you can have realtime feedback of the compilation.

## Get result form console

**Warning:** your can't use stdout by `console.log` or `stdout.write`, since coc has to use stdio for communication between neovim.

You can use `console.error` to write a string message, like:
``` js
console.error('my error')
```
the message will be echoed in vim. However, this method is quite limited.

## Using logger module.

The default log level is `info`, so you won't get `error` or `trace` messages.
To change the log level, run:

```
export NVIM_COC_LOG_LEVEL=debug
``` 
in your shell.

Or add:
``` vim
let $NVIM_COC_LOG_LEVEL = 'debug'
```
at the start of your `.vimrc` file.

Most files import logger module for debug purpose, like:
``` js
const logger = require('./util/logger')('workspace')
```
You can use the logger to debug any variable, like:
``` js
logger.debug('variable:', variable)
```
Use command `node -e 'console.log(path.join(os.tmpdir(), "coc-nvim.log"))'` to get the log file, and `tail` command to trace the log.

For extension, logger exists as `logger` property of `ExtensionContext`.

## Use chrome developer tools

Add:
```
  let g:coc_node_args = ['--nolazy', '--inspect-brk=6045']
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