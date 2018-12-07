# Contents

* [Supported features](https://github.com/neoclide/coc.nvim/wiki/Language-servers#supported-features)
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

  * [x] workspaceFolders
  * [x] didChangeWorkspaceFolders
  * [x] didChangeConfiguration
  * [x] configuration
  * [x] didChangeWatchedFiles
  * [x] symbol
  * [x] executeCommand
  * [x] applyEdit
  * [x] resource operations

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
  * [x] documentLink
  * [x] documentLink resolve
  * [x] documentColor
  * [x] colorPresentation
  * [x] formatting
  * [x] rangeFormatting
  * [x] onTypeFormatting
  * [x] prepare rename
  * [x] rename
  * [x] foldingRange

**Note:** different server could have different capabilities.

## Register custom language servers

User defined language servers are configured in `languageserver` field of [configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-configuration-file).

There're three types of language servers: `module`,`executable` and `socket`.

* `module` type language server are running by nodejs and using node IPC for connection.
* `executable` type language server are spawned with executable command while using stdio for connection.
* `socket` language server are started in separated process, normally used for debugging purpose.

Different language server type have different configuration schema.

An example of `module` language server:

``` jsonc
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

``` jsonc
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

``` jsonc
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

## Example configuration for custom language servers

Add `languageserver` section in your `coc-settings.json` for regist custom language servers.

### Dart

Using [natebosch/dart_language_server](https://github.com/natebosch/dart_language_server):

``` jsonc
  "languageserver": {
    "dart": {
      "command": "dart_language_server",
      "args": [],
      "filetypes": ["dart"],
      "initializationOptions": {},
      "settings": {
        "dart": {
          "validation": {},
          "completion": {}
        }
      }
    }
  }
```

### C/C++/Objective-C

Using [ccls](https://github.com/MaskRay/ccls)

``` jsonc
  "ccls": {
    "command": "ccls",
    "filetypes": ["c", "cpp", "objc", "objcpp"],
    "initializationOptions": {
      "cacheDirectory": "/tmp/ccls"
    }
  }
```

Using [cquery](https://github.com/cquery-project/cquery)

``` jsonc
  "languageserver": {
    "cquery": {
      "command": "cquery",
      "args": ["--log-file=/tmp/cq.log"],
      // disable automatic reveal output channel
      "revealOutputChannelOn": "never",
      "filetypes": ["c", "cpp"],
      "initializationOptions": {
        "cacheDirectory": "/tmp/cquery"
      }
    }
  }
```

Using [clangd](http://llvm.org/viewvc/llvm-project/clang-tools-extra/trunk/clangd/)

``` jsonc
  "languageserver": {
    "clangd": {
      "command": "clangd",
      "filetypes": ["c", "cpp", "objc", "objcpp"]
    }
  }
```

Like many tools, clangd relies on the presence of a [JSON compilation database](https://clang.llvm.org/docs/JSONCompilationDatabase.html)

### Go

Using [sourcegraph/go-langserver](https://github.com/sourcegraph/go-langserver)

``` jsonc
  "languageserver": {
    "golang": {
      "command": "go-langserver",
      "filetypes": ["go"],
      "revealOutputChannelOn": "never",
      "initializationOptions": {
        "gocodeCompletionEnabled": true,
        "diagnosticsEnabled": true,
        "lintTool": "golint"
      }
    }
  }
```

### PHP

Using [felixfbecker/php-language-server](https://github.com/felixfbecker/php-language-server)

``` jsonc
  "languageserver": {
    "phplang": {
      "command": "php",
      "args": ["vendor/felixfbecker/language-server/bin/php-language-server.php"],
      "filetypes": ["php"]
    }
  }
```

### Dockerfile

Using [rcjsuen/dockerfile-language-server-nodejs](https://github.com/rcjsuen/dockerfile-language-server-nodejs)

``` jsonc
  "languageserver": {
    "dockerfile": {
      "command": "docker-langserver",
      "filetypes": ["dockerfile"],
      "args": ["--stdio"]
    }
  }
```

### Bash

Using [mads-hartmann/bash-language-server](https://github.com/mads-hartmann/bash-language-server)

``` jsonc
  "languageserver": {
    "bash": {
      "command": "bash-language-server",
      "filetypes": ["sh"],
      "args": ["start"]
    }
  }
```

### Lua

Using [Alloyed/lua-lsp](https://github.com/Alloyed/lua-lsp)

``` jsonc
  "languageserver": {
    "lua": {
      "command": "lua-lsp",
      "filetypes": ["lua"]
    }
  }
```

### OCaml and ReasonML

Using [ocaml-language-server](https://github.com/freebroccolo/ocaml-language-server)

``` jsonc
  "languageserver": {
     "ocaml": {
       "command": "ocaml-language-server",
       "args": ["--stdio"],
       "filetypes": ["ocaml", "reason"]
     }
  }
```

Using [reason-language-server](https://github.com/jaredly/reason-language-server)

``` jsonc
  "languageserver": {
     "reason": {
      "command": "/absolute/path/to/reason-language-server",
      "filetypes": ["reason"]
     }
  }
```