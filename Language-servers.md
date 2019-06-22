# Contents

* [Supported features](https://github.com/neoclide/coc.nvim/wiki/Language-servers#supported-features)
* [Register custom language servers](https://github.com/neoclide/coc.nvim/wiki/Language-servers#register-custom-language-servers)

## Supported features

Check out the official specification at https://microsoft.github.io/language-server-protocol/specification.

**Note:** different servers can have different capabilities.

## Register custom language servers

User defined language servers are configured in the `languageserver` field of the [configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-the-configuration-file).

There're three types of language servers: `module`,`executable` and `socket`.

* `module` type language servers are run by nodejs and using node IPC for connection.
* `executable` type language servers are spawned with an executable command while using stdio for connection.
* `socket` language servers are started in a separated process, normally used for debugging purpose.

Different language server types have different configuration schema.

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

`port` is required for a socket service and user should start the socket server before coc starts.

* `initializationOptions` is the json object that passed to [language server on initialize](https://microsoft.github.io/language-server-protocol/specification#initialize).
* `settings` contains specific configuration of the language server.
* `trace.server` controls trace level of communication between server and client. The default is `"off"`. Change to `"verbose"` if you want to checkout all communication.
* `rootPatterns` is used to resolve the root path which should contain one of the patterns as a child directory or file, it will use `"coc.preferences.rootPatterns"` (default to `[".vim", ".git", ".hg", ".projections.json"]`)  when not specified,  
* `requireRootPattern` when this is true, the language server will only start when any matched rootPatterns found.

## Example configuration for custom language servers

Add `languageserver` section in your `coc-settings.json` for registering custom language servers.

### Dart

Using [natebosch/dart_language_server](https://github.com/natebosch/dart_language_server):

``` jsonc
  "languageserver": {
    "dart": {
      "command": "dart_language_server", // in windows is dart_language_server.bat
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
  "languageserver": {
    "ccls": {
      "command": "ccls",
      "filetypes": ["c", "cpp", "objc", "objcpp"],
      "rootPatterns": [".ccls", "compile_commands.json", ".vim/", ".git/", ".hg/"],
      "initializationOptions": {
         "cache": {
           "directory": "/tmp/ccls"
         }
       }
    }
  }
```
To make header completion work with clang < 8 on Mac OS X, use `"initializationOptions"` like:

``` jsonc
      "initializationOptions": {
        "cache": {
          "directory": "/tmp/ccls"
        },
        "clang": {
           // make sure you have installed commandLineTools
          "resourceDir": "/Library/Developer/CommandLineTools/usr/lib/clang/10.0.0",
          "extraArgs": [
            "-isystem",
            "/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include/c++/v1"，
            "-I",
            "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/"        
          ]
        }
      },
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

### Rust

You may simply install the [coc-rls](https://github.com/neoclide/coc-rls/) extension or try [rust-analyzer](https://github.com/rust-analyzer/rust-analyzer), which plans to replace RLS eventually:

``` jsonc
  "languageserver": {
    "rust": {
      "command": "ra_lsp_server",
      "filetypes": ["rust"],
      "rootPatterns": ["Cargo.toml"]
    }
  }
```

It's necessary to `rustup component add rust-src` and build `rust-analyzer` from sources (just `cargo install-lsp`).

### Go

Using [gopls](https://github.com/saibing/tools)

```jsonc
  "languageserver": {
    "golang": {
      "command": "gopls",
      "rootPatterns": ["go.mod", ".vim/", ".git/", ".hg/"],
      "filetypes": ["go"]
    }
  }
```

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

Try [marlonfan/coc-phpls](https://github.com/marlonfan/coc-phpls) or one of the following methods:

Using [bmewburn/intelephense-docs](https://github.com/bmewburn/intelephense-docs)

**Recommended** (way faster than php-language-server)

``` jsonc
{
    "languageserver": {
        "intelephense": {
            "command": "intelephense",
            "args": ["--stdio"],
            "filetypes": ["php"],
            "initializationOptions": {
                "storagePath": "/tmp/intelephense"
            }
        }
    },
}
```

Using [felixfbecker/php-language-server](https://github.com/felixfbecker/php-language-server)

``` jsonc
  "languageserver": {
    "phplang": {
      "command": "php",
      "args": ["/path/to/vendor/felixfbecker/language-server/bin/php-language-server.php"],
      "filetypes": ["php"]
    }
  }
```
note: make sure you can start the server by use command and args.

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

Using [sumneko/lua-language-server](https://github.com/sumneko/lua-language-server) on Windows, which can downloaded by the extension manager of VSCode.



```viml
let lua_lsp = glob('~/.vscode/extensions/sumneko.lua*', 0, 1)
if len(lua_lsp)
    let lua_lsp = lua_lsp[-1] . '\server'
    call coc#config('languageserver', {
        \ 'lua-language-server': {
        \     'cwd': lua_lsp,
        \     'command': lua_lsp . '\bin\lua-language-server.exe',
        \     'args': ['-E', '-e', 'LANG="zh-cn"', lua_lsp . '\main.lua'],
        \     'filetypes': ['lua'],
        \ }
    \ })
endif
```

Using [EmmyLua-LanguageServer](https://github.com/EmmyLua/EmmyLua-LanguageServer):

Make sure your Java environment variables are rights and change the path in the args field according to your installation.

``` jsonc
  "languageserver": {
    "lua": {
      "command": "java",
      "args": ["-cp", "/your/path/to/EmmyLua-LanguageServer/EmmyLua-LS/build/libs/EmmyLua-LS-all.jar", "com.tang.vscode.MainKt"],
      "filetypes": ["lua"],
      "rootPatterns": [".git/"]
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

### PureScript

Using [purescript-language-server](https://github.com/nwolverson/purescript-language-server)

``` jsonc
  "languageserver": {
     "purescript": {
       "command": "purescript-language-server",
       "args": ["--stdio"],
       "filetypes": ["purescript"],
       "rootPatterns": ["bower.json", "psc-package.json", "spago.dhall"]
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
    "initializationOptions": {},
    "settings": {
	"languageServerHaskell": {
		"hlintOn": false,
		"maxNumberOfProblems": 10,
		"completionSnippetsOn": true
	}
     }
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

### Elixir

Using [elixir-ls](https://github.com/JakeBecker/elixir-ls)

``` jsonc
  "languageserver": {
     "elixirLS": {
      "command": "/absolute/path/to/elixir-ls/release/language_server.sh",
      "filetypes": ["elixir", "eelixir"]
     }
  }
```

### Python

Use [coc-python](https://github.com/neoclide/coc-python) extension is recommended.

Make sure you've installed the [python-language-server](https://github.com/palantir/python-language-server)

Example with python-language-server (be careful not to condense the hierarchy as it breaks pyls)
```
"languageserver": {
  "python": {
    "command": "python",
    "args": [
      "-mpyls",
      "-vv",
      "--log-file",
      "/tmp/lsp_python.log"
    ],
    "trace.server": "verbose",
    "filetypes": [
      "python"
    ],
    "settings": {
      "pyls": {
        "enable": true,
        "trace": {
          "server": "verbose"
        },
        "commandPath": "",
        "configurationSources": [
          "pycodestyle"
        ],
        "plugins": {
          "jedi_completion": {
            "enabled": true
          },
          "jedi_hover": {
            "enabled": true
          },
          "jedi_references": {
            "enabled": true
          },
          "jedi_signature_help": {
            "enabled": true
          },
          "jedi_symbols": {
            "enabled": true,
            "all_scopes": true
          },
          "mccabe": {
            "enabled": true,
            "threshold": 15
          },
          "preload": {
            "enabled": true
          },
          "pycodestyle": {
            "enabled": true
          },
          "pydocstyle": {
            "enabled": false,
            "match": "(?!test_).*\\.py",
            "matchDir": "[^\\.].*"
          },
          "pyflakes": {
            "enabled": true
          },
          "rope_completion": {
            "enabled": true
          },
          "yapf": {
            "enabled": true
          }
        }
      }
    }
  }
}
```

### Ruby

Use [coc-solargraph](https://github.com/neoclide/coc-solargraph) extension is recommended.

Make sure solargraph is in your $PATH (sudo gem install solargraph) or use `solargraph.commandPath` to configure executable path of solargraph.


### Scala

Using [scalameta/metals](https://github.com/scalameta/metals):

Make sure the generated metals-vim binary is available on your $PATH. Installation instructions can be found on the [Scalameta Metals website](https://scalameta.org/metals/docs/editors/vim.html).

``` jsonc
  "languageserver": {
    "metals": {
      "command": "metals-vim",
      "rootPatterns": ["build.sbt"],
      "filetypes": ["scala", "sbt"]
    }
  }
```

### LaTeX

Using [astoff/digestif](https://github.com/astoff/digestif):

Make sure the digestif executable is available on your $PATH or use absolute path as command. Installation instructions can be found [here](https://github.com/astoff/digestif#installation-and-set-up).

``` jsonc
  "languageserver": {
    "digestif": {
      "command": "digestif",
      "filetypes": ["tex", "plaintex", "context"]
    }
  }
```

Using [Texlab](https://texlab.netlify.com/):

``` jsonc
  "languageserver": {
    "latex": {
      "command": "java",
      "args": ["-jar", "/PATH/TO/texlab.jar"],
      // not start server at home directory, since it won't work.
      "ignoredRootPaths": ["~"],
      "filetypes": ["tex", "bib", "plaintex", "context"]
    }
  }
```

* Make sure `java` in your $PAHT.
* May need to add `let g:tex_flavor = "latex"` to correct buffer filetype, check it by `:echo &filetype`.
* Adjsut the path to textlab.jar accordingly.
* For bibTeX integration, you should use package `biblatex`, check the gif on https://texlab.netlify.com/

### Elm

Using [elm-tooling/elm-language-server](https://github.com/elm-tooling/elm-language-server):

``` jsonc
  "languageserver": {
    "elmLS": {
      "command": "elm-language-server",
      "args": ["--stdio"],
      "filetypes": ["elm"],
      "rootPatterns": ["elm.json"],
      "initializationOptions": {
        "runtime": "node",
        "elmPath": "elm",
        "elmFormatPath": "elm-format",
        "elmTestPath": "elm-test"
      }
    }
  }
```

* Make sure `elm-language-server`, `elm`, `elm-format` and `elm-test` are in your $PATH.
* Checkout [github page](https://github.com/elm-tooling/elm-language-server#cocnvim) for more information.

### Fortran

Using [fortran-language-server](https://github.com/hansec/fortran-language-server):

Make sure the fortls executable is available on your $PATH.

``` jsonc
  "languageserver": {
     "fortran": {
       "command": "fortls",
       "filetypes": ["fortran"],
       "rootPatterns": [".fortls", ".git/"]
     }
  }
```
### Clojure

Using [clojure-lsp](https://github.com/snoe/clojure-lsp):

``` jsonc
  "languageserver": {
    "clojure-lsp": {
      "command": "bash",
      "args": ["-c", "clojure-lsp"],
      "filetypes": ["clojure"],
      "rootPatterns": ["project.clj"],
      "additionalSchemes": ["jar", "zipfile"],
      "trace.server": "verbose",
      "initializationOptions": {
      }
    }
  }
```

* Make sure `clojure-lsp` is in your $PATH.
* Checkout [github page](https://github.com/snoe/clojure-lsp#vim) for more information.

### Julia

Using [`LanguageServer.jl`](https://github.com/JuliaEditorSupport/LanguageServer.jl).

The `LanguageServer`, `SymbolServer` and `StaticLint` packages must be installed in Julia (1.x), i.e.

``` julia
julia> using Pkg
julia> Pkg.add("LanguageServer")
julia> Pkg.add("SymbolServer")
julia> Pkg.add("StaticLint")
```
Registering the server:

``` jsonc
  "languageserver": {
    "julia": {
      "command": "/usr/bin/julia",
      "args" : ["--startup-file=no", "--history-file=no", "-e",
      "using LanguageServer;\
       using Pkg;\
       import StaticLint;\
       import SymbolServer;\
       env_path = dirname(Pkg.Types.Context().env.project_file);\
       debug = false;\
       server = LanguageServer.LanguageServerInstance(stdin, stdout, debug, env_path, \"\", Dict());\
       server.runlinter = true;\
       run(server);"
       ],
      "filetypes": ["julia"]
     }
  }
```

Checkout [JuliaEditorSupport/LanguageServer.jl](https://github.com/JuliaEditorSupport/LanguageServer.jl/wiki/Vim-and-Neovim) for more information.

### R

Use [coc-r-lsp](https://github.com/neoclide/coc-r-lsp) extension.