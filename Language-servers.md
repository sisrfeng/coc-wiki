To make coc have best features like VSCode, coc bundles with some language server extensions itself, while users still able 
configure custom language servers.

# Contents

* [Supported features](https://github.com/neoclide/coc.nvim/wiki/Language-servers#supported-features)
* [Built in server extensions](https://github.com/neoclide/coc.nvim/wiki/Language-servers#built-in-server-extensions)
* [Register custom language servers](https://github.com/neoclide/coc.nvim/wiki/Language-servers#register-custom-language-servers)

## Supported features

Check out the official specification at https://microsoft.github.io/language-server-protocol/specification.

* General
  * [x] initialize
  * [x] initalized
  * [x] shutdown
  * [x] exit
  * [x] $/cancelRequest

* Window

  * [x] showMessage
  * [x] showMessageRequest
  * [x] logMessage

* Telemetry

  * [x] event (just log the event)

* Client

  * [x] registerCapability
  * [x] unregisterCapability

* Workspace

  * [ ] workspaceFolders
  * [ ] didChangeWorkspaceFolders
  * [x] didChangeConfiguration
  * [x] configuration
  * [x] didChangeWatchedFiles
  * [x] symbol
  * [x] executeCommand
  * [x] applyEdit

* Text Synchronization

  * [x] didOpen
  * [x] didChange
  * [x] willSave
  * [x] willSaveWaitUntil
  * [x] didSave
  * [x] didClose

* Diagnostics

  * [x] publishDiagnostics

* Language Features

  * [x] completion
  * [x] completion resolve
  * [x] hover
  * [x] signatureHelp
  * [x] definition
  * [x] typeDefinition
  * [x] implementation
  * [x] references
  * [x] documentHighlight
  * [x] documentSymbol
  * [x] codeAction
  * [x] codeLens
  * [x] codeLens resolve
  * [ ] documentLink
  * [ ] documentLink resolve
  * [ ] documentColor
  * [ ] colorPresentation
  * [x] formatting
  * [x] rangeFormatting
  * [x] onTypeFormatting
  * [x] rename
  * [ ] foldingRange

**Note:** different server could have different capabilities.

## Built in server extensions

You can find all the built extensions in `src/extensions`

Name         | File types              | Server/extension repository
------------ | -------------           |------------
tsserver     | typescript, javascript  | [typescript-language-features](https://github.com/Microsoft/vscode/tree/master/extensions/typescript-language-features)
html         | html, handlebars, razor | [vscode-html-languageserver-bin](https://www.npmjs.com/package/vscode-html-languageserver-bin)
css          | css, less, scss, wxss   | [css-language-features](https://github.com/Microsoft/vscode/tree/master/extensions/css-language-features)
eslint       | javascript              | [vscode-eslint](https://github.com/Microsoft/vscode-eslint)
json         | json, jsonc             | [vscode-json-languageserver](https://www.npmjs.com/package/vscode-json-languageserver)
stylelint    | css, wxss, scss...      | [vscode-stylelint](https://github.com/shinnn/vscode-stylelint)
tslint       | typescript, javascript  | [vscode-tslint](https://github.com/Microsoft/vscode-tslint)
wxml         | wxml                    | [wxml-langserver](https://github.com/chemzqm/wxml-languageserver)
solargraph   | ruby                    | [vscode-solargraph](https://github.com/castwide/vscode-solargraph)
vetur        | vue                     | [vetur](https://github.com/vuejs/vetur)
pyls         | python                  | [python-language-server](https://github.com/palantir/python-language-server)

For settings of built in extensions, check out [Extension configuration](https://github.com/neoclide/coc.nvim/wiki/Using-configuration-file#extension-configuration)

## Register custom language servers

User defined language servers are configured in `languageserver` field of [configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-configuration-file).

There're three types of language servers: `module`,`executable` and `socket`.

* `module` type language server are running by nodejs and using node IPC for connection.
* `executable` type language server are spawned with executable command while using stdio for connection.
* `socket` language server are started in separated process, normally used for debugging purpose.

Different language server type have different configuration schema.

An example of `module` language server:

``` json
  "languageserver": {
    "foo": {
      "module": "/usr/local/lib/node_modules/foo/index.js",
      "args": ["--node-ipc"],
      "filetypes": ["foo"],
      "cwd": "./src",
      // Used for debugging
      "execArgv": ["--nolazy", "--inspect-brk=6045"],
      "initializationOptions": {
      },
      "settings": {
        "validate": true
      }
    }
  }
```
`module` and `filetypes` are required for module language server.

An example of `executable` language server:

``` json
  "languageserver": {
    "bar": {
      "command": "bar",
      "args": ["--stdio"],
      "filetypes": ["bar"],
      "cwd": "./src",
      "initializationOptions": {
      },
      "settings": {
      }
    }
  }
```
`command` and `filetypes` are required for executable language server.

An example of socket language server:

``` json
"languageserver": {
    "socketserver": {
      "host": "127.0.0.1",
      "port": 9527
    }
  }
```

`port` is required for socket service and user should start the socket server before coc started.

* `initializationOptions` is the json object that passed to [language server on initialize](https://microsoft.github.io/language-server-protocol/specification#initialize).
* `settings` contains specific configuration of language server.
* `cwd` could be path that relative from workspace root (directory contains `.vim` or `.git`).