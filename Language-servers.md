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
      "trace.server": "verbose",
      "rootPatterns": ["root.yml"],
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
* `trace.server` controls trace level of communication between server and client, default `"off"`, change to `"verbose"` if you want to checkout all communication.
* `rootPatterns` is used for resolve root path which should contain one of patterns as child directory or file, it will use `g:rooter_patterns` (default to `['.vim/', '.git/', '.hg/', '.projections.json']`)  when not specified,  

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
    "rootPatterns": [".ccls", "compile_commands.json", ".vim/", ".git/", ".hg/"],
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
      "filetypes": ["c", "cpp"],
      "rootPatterns": ["compile_flags.txt", "compile_commands.json", ".vim/", ".git/", ".hg/"],
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
      "rootPatterns": ["compile_flags.txt", "compile_commands.json", ".vim/", ".git/", ".hg/"],
      "filetypes": ["c", "cpp", "objc", "objcpp"]
    }
  }
```

Like many tools, clangd relies on the presence of a [JSON compilation database](https://clang.llvm.org/docs/JSONCompilationDatabase.html)

### Go

Using [saibing/bingo](https://github.com/saibing/bingo)

``` jsonc
  "languageserver": {
    "golang": {
      "command": "bingo",
      "args": ["--diagnostics-style=instant"],
      "rootPatterns": ["go.mod", ".vim/", ".git/", ".hg/"],
      "filetypes": ["go"]
    }
  }
```

Using [sourcegraph/go-langserver](https://github.com/sourcegraph/go-langserver)

``` jsonc
  "languageserver": {
    "golang": {
      "command": "go-langserver",
      "filetypes": ["go"],
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
      "args": ["start"],
      "filetypes": ["sh"],
      "ignoredRootPaths": ["~"]
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

Using [reason-language-server](https://github.com/jaredly/reason-language-server#what-about-the-ocaml-language-server)

``` jsonc
  "languageserver": {
     "reason": {
      "command": "/absolute/path/to/reason-language-server.exe",
      "filetypes": ["reason"]
     }
  }
```

### Flow

Using [flow-language-server](https://github.com/flowtype/flow-language-server), note: flow-language-server is no longer maintained

``` jsonc
  // disable tsserver for javascript
  "tsserver.enableJavascript": true,
  "languageserver": {
    "flow": {
      "command": "flow-language-server",
      "args": ["--stdio"],
      "filetypes": ["javascript", "javascriptreact"],
      "rootPatterns": [".flowconfig"]
    },
  }
```

Using [flow lsp](https://github.com/facebook/flow)

```jsonc
  "languageserver": {
    "flow": {
      "command": "flow",
      "args": ["lsp"],
      "filetypes": ["javascript", "javascriptreact"],
      "initializationOptions": {},
      "requireRootPattern": true,
      "settings": {},
      "rootPatterns": [".flowconfig"]
    }
  },
```

### Haskell

Using [Haskell IDE Engine](https://github.com/haskell/haskell-ide-engine)

```jsonc
"languageserver": {
  "haskell": {
    "command": "hie-wrapper",
    "rootPatterns": [".stack.yaml", "cabal.config", "package.yaml"],
    "filetypes": ["hs", "lhs", "haskell"],
    "initializationOptions": {}
  }
}
```

### vim/erb/markdown

Using [efm-langserver](https://github.com/mattn/efm-langserver)

Location of efm-langserver config.yaml is:

- UNIX: `$HOME/.config/efm-langserver/config.yaml`
- Windows: `%APPDATA%\efm-langserver\config.yaml`

efm-langserver config:

```yaml
languages:
  eruby:
    lint-command: 'erb -x -T - | ruby -c'
    lint-stdin: true
    lint-offset: 1
    format-command: 'htmlbeautifier'

  vim:
    lint-command: 'vint -'
    lint-stdin: true

  markdown:
    lint-command: 'markdownlint -s'
    lint-stdin: true
    lint-formats:
      - '%f: %l: %m'
```

coc-settings.json
```jsonc
  "languageserver": {
    "efm": {
      "command": "efm-langserver",
      "args": [],
      // custom config path
      // "args": ["-c", "/path/to/your/config.yaml"],
      "filetypes": ["vim", "eruby", "markdown"]
    }
  }
```