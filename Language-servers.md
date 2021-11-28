# Contents

- [Supported features](#supported-features)
- [Register custom language servers](#register-custom-language-servers)
- [Example language server configuration](#example-language-server-configuration)
  - [Ada/SPARK](#adaspark)
  - [Arduino](#Arduino)
  - [Bash](#bash)
  - [C/C++/Objective-C](#ccobjective-c)
  - [CMake](#cmake)
  - [Clojure](#clojure)
  - [Common Lisp](#Common-Lisp)
  - [Crystal](#crystal)
  - [Css/Less/Sass](#csslesssass)
  - [Dart](#dart)
  - [D](#d)
  - [Deno](#deno)
  - [Dhall](#dhall)
  - [Dockerfile](#dockerfile)
  - [Elixir](#elixir)
  - [Elm](#elm)
  - [Flow](#flow)
  - [Fortran](#fortran)
  - [FSharp](#fsharp)
  - [Go](#go)
  - [Godot](#godot)
  - [GraphQL](#graphql)
  - [Groovy](#groovy)
  - [Haskell](#haskell)
  - [Html](#html)
  - [Javascript/Typescript](#javascripttypescript)
  - [Json](#json)
  - [Julia](#julia)
  - [Kotlin](#kotlin)
  - [LaTeX](#latex)
  - [Lua](#lua)
  - [Markdown](#markdown)
  - [Nim](#nim)
  - [Nix](#nix)
  - [OCaml and ReasonML](#ocaml-and-reasonml)
  - [Perl](#perl)
  - [PHP](#php)
  - [PureScript](#purescript)
  - [Python](#python)
  - [R](#r)
  - [Racket](#racket)
  - [Robot Framework](#robot-framework)
  - [Rome](#rome)
  - [Ruby](#ruby)
  - [Rust](#rust)
  - [SQL](#sql)
  - [Scala](#scala)
  - [SystemVerilog](#systemverilog)
  - [Terraform](#terraform)
  - [Vala](#vala)
  - [Vue](#vue)
  - [Zig](#zig)
  - [vim/erb/markdown](#vimerbmarkdown)
    - [Using Sorbet:](#using-sorbet)

## Supported features

Check out the official specification at https://microsoft.github.io/language-server-protocol/specifications/specification-3-15/.

**Note:** different servers can have different capabilities.

## Register custom language servers

User defined language servers are configured in the `languageserver` field of the [configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-the-configuration-file). Checkout `:h coc-config-languageserver`
for common settings of language server configuration.

There are three types of language servers: `module`, `executable` and `socket`.

- `module` type language servers are run by nodejs and using node IPC for connection.
- `executable` type language servers are spawned with an executable command while using stdio for connection.
- `socket` language servers are started in a separated process, normally used for debugging purpose.

Different language server types have different configuration schema.

An example of `module` language server:

```jsonc
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

```jsonc
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

```jsonc
"languageserver": {
    "socketserver": {
      "host": "127.0.0.1",
      "port": 9527
    }
  }
```

`port` is required for a socket service and user should start the socket server before coc starts.

Install [coc-json](https://github.com/neoclide/coc-json) for completion and validation support.

## Example language server configuration

Add `languageserver` section in your `coc-settings.json` for registering custom language servers.

### Ada/SPARK

Using [Ada Language Server](https://github.com/AdaCore/ada_language_server).
See installation instructions on the Github homepage of this LSP.

```jsonc
"languageserver": {
  "Ada": {
    "command": "path/to/ada_language_server",
    "filetypes": ["ada"]
  }
}
```

### Arduino
Using [Arduino Language Server](https://github.com/arduino/arduino-language-server). 
See the installation instructions in the README file of this LSP.

```jsonc
"languageserver":{
  "arduino":{ 
    "command":"/path/to/arduino-language-server",
    "rootPatterns":["*.ino"],
    "filetypes":["arduino"],
    "args":["-cli", "/path/to/arduino-cli", "-clangd", "/path/to/clangd", "-cli-config", "/path/to/arduino-cli.yaml"]
  }
}
```

Tips: 
  - This LSP requires `clangd` and `arduino-cli` to work properly. Modify the values according to the locations of the binaries on your machine.
  - To determine (or generate if there isn't one) the location of `arduino-cli.yaml`, refer to [this page](https://arduino.github.io/arduino-cli/latest/configuration/).

### Bash

Via [coc-sh](https://github.com/josa42/coc-sh) or

Using [mads-hartmann/bash-language-server](https://github.com/mads-hartmann/bash-language-server)

```jsonc
"languageserver": {
  "bash": {
    "command": "bash-language-server",
    "args": ["start"],
    "filetypes": ["sh"]
  }
}
```

### C/C++/Objective-C

Using [clangd](https://clang.llvm.org/extra/clangd/Installation.html) with [coc-clangd](https://github.com/clangd/coc-clangd) or:

```jsonc
"languageserver": {
  "clangd": {
    "command": "clangd",
    "rootPatterns": ["compile_flags.txt", "compile_commands.json"],
    "filetypes": ["c", "cc", "cpp", "c++", "objc", "objcpp"]
  }
}
```

Like many tools, clangd relies on the presence of a [JSON compilation database](https://clang.llvm.org/docs/JSONCompilationDatabase.html)

Using [ccls](https://github.com/MaskRay/ccls)

```jsonc
"languageserver": {
  "ccls": {
    "command": "ccls",
    "filetypes": ["c", "cc", "cpp", "c++", "objc", "objcpp"],
    "rootPatterns": [".ccls", "compile_commands.json", ".git/", ".hg/"],
    "initializationOptions": {
        "cache": {
          "directory": "/tmp/ccls"
        }
      }
  }
}
```

To make header completion work with clang < 8 on Mac OS X, use `"initializationOptions"` like:

```jsonc
"initializationOptions": {
  "cache": {
    "directory": "/tmp/ccls"
  },
  "clang": {
      // make sure you have installed commandLineTools
    "resourceDir": "/Library/Developer/CommandLineTools/usr/lib/clang/11.0.0",
    "extraArgs": [
      "-isystem",
      "/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include/c++/v1",
      "-I",
      "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/"
    ]
  }
},
```

Using [cquery](https://github.com/cquery-project/cquery)

```jsonc
"languageserver": {
  "cquery": {
    "command": "cquery",
    "args": ["--log-file=/tmp/cq.log"],
    "filetypes": ["c", "cc", "cpp", "c++"],
    "rootPatterns": ["compile_flags.txt", "compile_commands.json", ".git/", ".hg/"],
    "initializationOptions": {
      "cacheDirectory": "/tmp/cquery"
    }
  }
}
```

### CMake

Try [coc-cmake](https://github.com/voldikss/coc-cmake) (not implemented with LSP) or

Using [cmake-language-server](https://github.com/regen100/cmake-language-server)

```jsonc
"languageserver": {
  "cmake": {
    "command": "cmake-language-server",
    "filetypes": ["cmake"],
    "rootPatterns": [
      "build/"
    ],
    "initializationOptions": {
      "buildDirectory": "build"
    }
  }
}
```
### Common Lisp

Use [`coc-cl`](https://github.com/UltiRequiem/coc-cl)
```
:CocInstall coc-cl
```

### Clojure

Using [clojure-lsp](https://github.com/snoe/clojure-lsp)

```jsonc
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

- Make sure `clojure-lsp` is in your \$PATH.
- Check out [github page](https://github.com/snoe/clojure-lsp#vim) for more information.

### Crystal

Using [`crystalline`](https://github.com/elbywan/crystalline). Follow the instructions to install it. 

```jsonc
  "languageserver": {
    "crystal": {
      "command": "crystalline",
      "args": [
        "--stdio"
      ],
      "filetypes": [
        "crystal"
      ]
    }
  }
```
Make sure you have `crystalline` available in your PATH

### Css/Less/Sass

Use [coc-css](https://github.com/neoclide/coc-css/issues/21) is recommended.

### Dart

On option is use [coc-flutter](https://github.com/iamcco/coc-flutter), that leverages [analysis_server](https://github.com/dart-lang/sdk/blob/master/pkg/analysis_server/tool/lsp_spec/README.md) from [dart-sdk](https://github.com/dart-lang/sdk).

Another option is configure analysis_server yourself. Use analysis_server from [dart-sdk](https://github.com/dart-lang/sdk):

```jsonc
"languageserver": {
  "dart": {
    "command": "dart",
    "args": [
      " change this to the path of analysis_server
      "/usr/local/opt/dart/libexec/bin/snapshots/analysis_server.dart.snapshot",
      "--lsp",
      "--client-id",
      "vim",
      "--client-version",
      "coc.nvim",
    ],
    "filetypes": ["dart"],
    "trace.server": "verbose"
  },
}
```

Or use [natebosch/dart_language_server](https://github.com/natebosch/dart_language_server)

```jsonc
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

### D

Use [`coc-dlang`](https://github.com/vushu/coc-dlang)
```
:CocInstall coc-dlang
```

### Deno

Use [`coc-deno`](https://github.com/fannheyward/coc-deno)

```
:CocInstall coc-deno
```

### Dhall

Using [`dhall-lsp-server`](https://github.com/dhall-lang/dhall-haskell/tree/master/dhall-lsp-server). Follow the instructions to install it.

```jsonc
"languageserver": {
  "dhall": {
    "command": "dhall-lsp-server",
    "filetypes": [
      "dhall"
    ]
  }
}
```

### Dockerfile

Using [rcjsuen/dockerfile-language-server-nodejs](https://github.com/rcjsuen/dockerfile-language-server-nodejs)

```jsonc
"languageserver": {
  "dockerfile": {
    "command": "docker-langserver",
    "filetypes": ["dockerfile"],
    "args": ["--stdio"]
  }
}
```

### Elixir

Using [elixir-ls](https://github.com/elixir-lsp/elixir-ls)

If you want to use @spec suggestions you have to enable codelens.

```jsonc
"codeLens.enable": true,
"languageserver": {
  "elixirLS": {
    "command": "/absolute/path/to/elixir-ls/release/language_server.sh",
    "filetypes": ["elixir", "eelixir"]
  }
}
```

### Elm

Using [elm-tooling/elm-language-server](https://github.com/elm-tooling/elm-language-server)

```jsonc
"languageserver": {
  "elmLS": {
    "command": "elm-language-server",
    "filetypes": ["elm"],
    "rootPatterns": ["elm.json"],
    "initializationOptions": {
      "elmPath": "elm", // optional
      "elmFormatPath": "elm-format", // optional
      "elmTestPath": "elm-test", // optional
      "elmAnalyseTrigger": "change" // optional
    }
  }
}
```

`elm-language-server` needs [`elm`](https://guide.elm-lang.org/install.html), [`elm-format`](https://github.com/avh4/elm-format) and [`elm-test`](https://github.com/elm-explorations/test)

- If a path like `elmPath` is defined, it'll be used.
- If a path is missing, `elm-language-server` will try to load it from a local npm installation folder. Otherwise a global installation provided via `PATH` is used.
- `elmAnalyseTrigger`: `elm-analyse` (linting) is executed on 'change', 'save' or 'never' (default: 'change')

Check out [github page](https://github.com/elm-tooling/elm-language-server#cocnvim) for more information.

### Flow

Using [flow-language-server](https://github.com/flowtype/flow-language-server) (Note: project no longer maintained)

```jsonc
// disable tsserver for javascript
"tsserver.enableJavascript": false,
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

### Fortran

Using [fortran-language-server](https://github.com/hansec/fortran-language-server)

Make sure the fortls executable is available on your \$PATH.

```jsonc
"languageserver": {
  "fortran": {
    "command": "fortls",
    "filetypes": ["fortran"],
    "rootPatterns": [".fortls", ".git/"]
  }
}
```

### FSharp

Install [FsAutoComplete](https://github.com/fsharp/FsAutoComplete)

`dotnet tool install --global fsautocomplete`

Associate F# file extensions with fsharp

`autocmd BufNewFile,BufRead *.fs,*.fsx,*.fsi set filetype=fsharp`

```jsonc
"languageserver": {
  "fsharp": {
    "command": "fsautocomplete",
    "args": ["--background-service-enabled"],
    "filetypes": ["fsharp"],
    "trace.server": "verbose",
    "initalizationOptions": {
      "AutomaticWorkspaceInit": true
    },
    "settings": {
      "FSharp.keywordsAutocomplete": true,
      "FSharp.ExternalAutocomplete": false,
      "FSharp.Linter": true,
      "FSharp.UnionCaseStubGeneration": true,
      "FSharp.UnionCaseStubGenerationBody": "failwith \"Not Implemented\"",
      "FSharp.RecordStubGeneration": true,
      "FSharp.RecordStubGenerationBody": "failwith \"Not Implemented\"",
      "FSharp.InterfaceStubGeneration": true,
      "FSharp.InterfaceStubGenerationObjectIdentifier": "this",
      "FSharp.InterfaceStubGenerationMethodBody": "failwith \"Not Implemented\"",
      "FSharp.UnusedOpensAnalyzer": true,
      "FSharp.UnusedDeclarationsAnalyzer": true,
      "FSharp.UseSdkScripts": true,
      "FSharp.SimplifyNameAnalyzer": false,
      "FSharp.ResolveNamespaces": true,
      "FSharp.EnableReferenceCodeLens": true
    }
  }
}
```

### Go

Via [coc-go](https://github.com/josa42/coc-go) or

Using [gopls](https://github.com/golang/tools/tree/master/gopls)

```jsonc
"languageserver": {
  "golang": {
    "command": "gopls",
    "rootPatterns": ["go.mod"],
    "filetypes": ["go"]
  }
}
```

If want more debug，add` "args": ["serve", "-debug", "0.0.0.0:8484", "-rpc.trace"],` below `command`.

Using [sourcegraph/go-langserver](https://github.com/sourcegraph/go-langserver)

```jsonc
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

### Godot

Using [`coc-godot`](https://github.com/j3d42/coc-godot).

### GraphQL

Using [graphql-language-service-cli](https://www.npmjs.com/package/graphql-language-service-cli)

```jsonc
"languageserver": {
  "graphql": {
    "command": "graphql-lsp",
    "args": ["server", "-m", "stream"],
    // customize filetypes to your needs
    "filetypes": ["typescript", "typescriptreact", "graphql"]
  }
}
```

### Groovy

Using [`Language server for Groovy`](https://github.com/prominic/groovy-language-server). Check out sources, build it and place `groovy-language-server-all.jar` to any folder.

```jsonc
"languageserver": {
  "groovy": {
    "command": "java",
    "args" : ["-jar", "/path/to/groovy-language-server-all.jar"],
    "filetypes": ["groovy"]
  }
}
```

### Haskell

Using [Haskell Language Server](https://github.com/haskell/haskell-language-server)

```jsonc
"languageserver": {
  "haskell": {
    "command": "haskell-language-server-wrapper",
    "args": ["--lsp"],
    "rootPatterns": ["*.cabal", "stack.yaml", "cabal.project", "package.yaml", "hie.yaml"],
    "filetypes": ["haskell", "lhaskell"]
  }
}
```

- You may also opt to use `ghcup` to easily install latest `ghc`, `cabal` and `hls` binaries to your `PATH`.
- Check [HLS README](https://github.com/haskell/haskell-language-server#features) about global cabal configuration to enable documentation on hover.

With this you can avoid building anything from scratch and can start coding Haskell files right away.

Using [ghcide](https://github.com/haskell/ghcide) with `stack exec`

1. Build `ghcide` with the `copy-compiler-tool` flag i.e. Instead of using
   `stack install ghcide` do `$ stack build --copy-compiler-tool ghcide`.
   ([Read why `copy-compiler-tool` is preferred over
   `install`](https://lexi-lambda.github.io/blog/2018/02/10/an-opinionated-guide-to-haskell-in-2018/))

2. This step is a necessary workaround for `coc-settings.json`. Create a script file with name
   `ghcide-lsp-via-stack-exec` with this single line as content: `stack exec ghcide -- --lsp`. Make that script file an executable with `$ chmod u+x ghcide-lsp-via-stack-exec`. Place that executable script file somewhere in your
   path. You'd know this step went well if you don't see a `command not found` error when running that
   script file from a prompt like so `$ ghcide-lsp-via-stack-exec`.

3. Update your `coc-settings.json`:

   ```jsonc
   "languageserver": {
     "haskell": {
       "command": "ghcide-lsp-via-stack-exec",
       "args": [ "" ],
       "rootPatterns": [ "*.cabal", "cabal.config", "cabal.project", "package.yaml", "stack.yaml" ],
       "filetypes": [ "hs", "lhs", "haskell" ],
       "initializationOptions": { "languageServerHaskell": { "hlintOn": true }
       }
     },
     //...
   }
   ```

Using [Haskell IDE Engine](https://github.com/haskell/haskell-ide-engine)

```jsonc
"languageserver": {
  "haskell": {
    "command": "hie-wrapper",
    "args": ["--lsp"],
    "rootPatterns": [
      "stack.yaml",
      "cabal.config",
      "package.yaml"
    ],
    "filetypes": [
      "haskell",
      "lhaskell"
    ],
    "initializationOptions": {
      "languageServerHaskell": {
        "hlintOn": true
      }
    }
  }
}
```

### Html

Use [coc-html](https://github.com/neoclide/coc-html) is recommended.

### Javascript/Typescript

Use [coc-tsserver](https://github.com/neoclide/coc-tsserver) is recommended.

### Json

use [coc-json](https://github.com/neoclide/coc-json) is recommended.

### Julia

Using [`LanguageServer.jl`](https://github.com/JuliaEditorSupport/LanguageServer.jl)

The `LanguageServer`, `SymbolServer` and `StaticLint` packages must be installed in Julia (1.x), i.e.

```julia
julia> using Pkg
julia> Pkg.add("LanguageServer")
julia> Pkg.add("SymbolServer")
julia> Pkg.add("StaticLint")
```

Install [coc-julia](https://github.com/fannheyward/coc-julia), or register the server in `coc-settings.json`:

```jsonc
"languageserver": {
  "julia": {
    "command": "/usr/bin/julia",
    "args" : ["--startup-file=no", "--history-file=no", "-e",
    "using LanguageServer;\n       using Pkg;\n       import StaticLint;\n       import SymbolServer;\n       env_path = dirname(Pkg.Types.Context().env.project_file);\n       debug = false;\n       server = LanguageServer.LanguageServerInstance(stdin, stdout, debug, env_path, \"\");\n       server.runlinter = true;\n       run(server);" ],
    "filetypes": ["julia"]
  }
}
```

Check out [JuliaEditorSupport/LanguageServer.jl](https://github.com/JuliaEditorSupport/LanguageServer.jl/wiki/Vim-and-Neovim) for more information.

In case you hit [issue #836](https://github.com/julia-vscode/LanguageServer.jl/issues/836), the language server needs to be installed from master to avoid the problem:

```julia
julia> using Pkg
julia> Pkg.add(url="https://github.com/julia-vscode/LanguageServer.jl")
julia> Pkg.add("SymbolServer")
julia> Pkg.add("StaticLint")
```

### Kotlin

Using [kotlin-language-server](https://github.com/fwcd/kotlin-language-server)

1. Download server.zip from the [releases page](https://github.com/fwcd/kotlin-language-server/releases).
2. Unzip the file in a convenient directory, for example inside `~/lsp/kotlin/`.

```jsonc
"languageserver": {
  "kotlin": {
    "command": "~/lsp/kotlin/server/bin/kotlin-language-server",
    "filetypes": ["kotlin"]
  }
}
```

### LaTeX

Using [astoff/digestif](https://github.com/astoff/digestif)

Make sure the digestif executable is available on your \$PATH or use absolute path as command. Installation instructions can be found [here](https://github.com/astoff/digestif#installation-and-set-up).

```jsonc
"languageserver": {
  "digestif": {
    "command": "digestif",
    "filetypes": ["tex", "plaintex", "context"]
  }
}
```

For [Texlab](https://texlab.netlify.com/), use [coc-texlab](https://github.com/fannheyward/coc-texlab) or:

```jsonc
"languageserver": {
  "latex": {
    "command": "/PATH/TO/texlab",
    "filetypes": ["tex", "bib", "plaintex", "context"]
  }
}
```

- May need to add `let g:tex_flavor = "latex"` to correct buffer filetype, check it by `:echo &filetype`.
- Adjust the path to textlab accordingly, or simply use as command name, from `PATH`.
- For bibTeX integration, you should use package `biblatex`, check the gif on https://texlab.netlify.com/

### Lua

Using [Alloyed/lua-lsp](https://github.com/Alloyed/lua-lsp)

```jsonc
"languageserver": {
  "lua": {
    "command": "lua-lsp",
    "filetypes": ["lua"]
  }
}
```

Using [sumneko/lua-language-server](https://github.com/sumneko/lua-language-server). It's a little difficult to use `sumneko/lua-language-server` in coc.nvim directly, it's recommend to use [coc-sumneko-lua](https://github.com/xiyaowong/coc-sumneko-lua) that can download and setup for Lua. If you want to setup `sumneko/lua-language-server` in coc-settings.json, follow the steps: (tests on macOS, change the path if you're using Windows)

1. install `sumneko.lua` in VSCode
2. `cd ~/.vscode/extensions/sumneko.lua-2.3.7`
3. `chmod +x ./server/bin/macOS/lua-language-server`, have no idea why this is needed, because in my VSCode extension, the server is not executable.
4. setup in your coc-settings.json:

```json
{
  "languageserver": {
    "lua": {
      "command": "~/.vscode/extensions/sumneko.lua-2.3.7/server/bin/macOS/lua-language-server",
      "args": ["-E", "~/.vscode/extensions/sumneko.lua-2.3.7/server/main.lua"],
      "rootPatterns": [".git"],
      "filetypes": ["lua"]
    }
  }
}
```

Using [EmmyLua-LanguageServer](https://github.com/EmmyLua/EmmyLua-LanguageServer)

Make sure your Java environment variables are rights and change the path in the args field according to your installation.

```jsonc
"languageserver": {
  "lua": {
    "command": "java",
    "args": ["-cp", "/your/path/to/EmmyLua-LanguageServer/EmmyLua-LS/build/libs/EmmyLua-LS-all.jar", "com.tang.vscode.MainKt"],
    "filetypes": ["lua"],
    "rootPatterns": [".git/"]
  }
}
```

### Markdown

Using [coc-markdownlint](https://github.com/fannheyward/coc-markdownlint), which has a codeAction support to autofix errs.

    :CocInstall coc-markdownlint

### Nim

Using [`nimlsp`](https://github.com/PMunch/nimlsp). Follow the instructions in the README of the repository. (Make sure that your Nimble bin directory is in your path)

```jsonc
"languageserver": {
  "nim": {
    "command": "nimlsp",
    "filetypes": ["nim"]
  }
}
```

### Nix

Using [rnix-lsp](https://github.com/nix-community/rnix-lsp)

```jsonc
"languageserver": {
  "nix": {
    "command": "rnix-lsp",
    "filetypes": ["nix"]
  }
}
```

### OCaml and ReasonML

Using [ocaml-language-server](https://github.com/ocaml-lsp/ocaml-language-server)

```jsonc
"languageserver": {
    "ocaml": {
      "command": "ocaml-language-server",
      "args": ["--stdio"],
      "filetypes": ["ocaml", "reason"]
    }
}
```

If you installed merlin with opam

```jsonc
"languageserver": {
  "ocaml": {
    "command": "opam",
    "args": ["config", "exec", "--", "ocaml-language-server", "--stdio"],
    "filetypes": ["ocaml", "reason"]
  }
}
```

Using [ocaml-lsp](https://github.com/ocaml/ocaml-lsp) and opam

```jsonc
"languageserver": {
  "ocaml-lsp": {
    "command": "opam",
    "args": ["config", "exec", "--", "ocamllsp"],
    "filetypes": ["ocaml", "reason"]
  }
}
```

Using [ocaml-lsp](https://github.com/ocaml/ocaml-lsp) and esy

```jsonc
"languageserver": {
  "ocaml-lsp": {
    "command": "esy",
    "args": ["sh", "-c", "ocamllsp"],
    "filetypes": ["ocaml", "reason"]
  }
}
```

Using [reason-language-server](https://github.com/jaredly/reason-language-server#what-about-the-ocaml-language-server)

```jsonc
"languageserver": {
  "reason": {
    "command": "reason-language-server",
    "filetypes": ["reason"]
  }
}
```

### Perl

Use [coc-perl](https://github.com/bmeneg/coc-perl) is recommended.

### PHP

Try [marlonfan/coc-phpls](https://github.com/marlonfan/coc-phpls) or one of the following methods:

Using [bmewburn/intelephense-docs](https://github.com/bmewburn/intelephense-docs)

**Recommended** (way faster than php-language-server)

```jsonc
"languageserver": {
  "intelephense": {
    "command": "intelephense",
    "args": ["--stdio"],
    "filetypes": ["php"],
    "initializationOptions": {
      "storagePath": "/tmp/intelephense"
    }
  }
}
```

Using [felixfbecker/php-language-server](https://github.com/felixfbecker/php-language-server)

```jsonc
"languageserver": {
  "phplang": {
    "command": "php",
    "args": ["/path/to/vendor/felixfbecker/language-server/bin/php-language-server.php"],
    "filetypes": ["php"]
  }
}
```

Note: make sure you can start the server by use command and args.

Via [coc-psalm](https://github.com/yaegassy/coc-psalm) or

Using [vimeo/psalm](https://github.com/vimeo/psalm) (psalm-language-server)

```jsonc
"languageserver": {
  "psalmls": {
    "command": "vendor/bin/psalm-language-server",
    "filetypes": ["php"],
    "rootPatterns": ["psalm.xml", "psalm.xml.dist"],
    "requireRootPattern": true
  }
}
```

### PureScript

Using [purescript-language-server](https://github.com/nwolverson/purescript-language-server) ([Configuration](https://github.com/nwolverson/purescript-language-server#config))

```jsonc
"languageserver": {
  "purescript": {
    "command": "purescript-language-server",
    "args": ["--stdio"],
    "filetypes": ["purescript"],
    "rootPatterns": ["bower.json", "psc-package.json", "spago.dhall"]
  }
}
```

### Python

Use one of them below:

If you're using python3, consider use [coc-pyright](https://github.com/fannheyward/coc-pyright).

If you're using jedi, consider use [coc-jedi](https://github.com/pappasam/coc-jedi).

[coc-python](https://github.com/neoclide/coc-python) can be used as well, but the code is not activaly maintained.

Or configure [[python-language-server](https://github.com/palantir/python-language-server)] like:

```jsonc
// be careful not to condense the hierarchy as it breaks pyls
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

### R

First install language server from `R`:

```r
install.packages("languageserver")
```

Use [coc-r-lsp](https://github.com/neoclide/coc-r-lsp) extension.

If [coc-r-lsp](https://github.com/neoclide/coc-r-lsp) does not work properly, according to instructions from [REditorSupport/languageserver](https://github.com/REditorSupport/languageserver), add below languageserver object to configuration file:

```jsonc
"languageserver": {
    "R": {
        "enable": true,
        "command": "/usr/bin/R",
        "args": [
            "--slave",
            "-e",
            "languageserver::run()"
        ],
        "filetypes": [
            "r"
        ]
    }
}
```

### Robot Framework

- install [robotframework-lsp](https://pypi.org/project/robotframework-lsp/)
- Configure robot filetype detection in neovim configuration file `autocmd BufNewFile,BufRead *.robot setlocal filetype=robot`

```jsonc
"languageserver": {
    "robotframework_ls": {
        "command": "robotframework_ls",
        "filetypes": ["robot"],
        "settings": {
         // here the ls configuration
         }
    }
}
```
### Racket

Using the [racket-langserver](https://github.com/jeapostrophe/racket-langserver)

- Install racket-langserver `raco pkg install racket-langserver`
- Add the below snippet to your coc config

```jsonc
  "languageserver": {
    "racket": {
      "command": "racket",
      "args": [
        "--lib",
        "racket-langserver"
      ],
      "filetypes": [
        "scheme"
      ]
    }
  }
```

### Rome

- install [`coc-rome`](https://github.com/fannheyward/coc-rome) which uses rome
- or try [`rome`](https://github.com/romefrontend/rome) without an extension:

```jsonc
"languageserver": {
  "rome-lsp": {
    "command": "rome",
    "args": ["lsp"],
    "filetypes": [
      "javascript",
      "javascriptreact",
      "typescript",
      "typescriptreact",
      "json"
    ],
    "rootPatterns": [".config"],
    "requireRootPattern": true
  }
}
```

### Ruby

Using [coc-solargraph](https://github.com/neoclide/coc-solargraph)

Make sure solargraph is in your \$PATH (sudo gem install solargraph) or use `solargraph.commandPath` to configure executable path of solargraph.

### Rust

- install [coc-rust-analyzer](https://github.com/fannheyward/coc-rust-analyzer) which uses `rust-analyzer`
- or install [coc-rls](https://github.com/neoclide/coc-rls/) which uses `rls`
- or try [rust-analyzer](https://github.com/rust-analyzer/rust-analyzer) without an extension:

```jsonc
"languageserver": {
  "rust": {
    "command": "rust-analyzer",
    "filetypes": ["rust"],
    "rootPatterns": ["Cargo.toml"]
  }
}
```

It's necessary to `rustup component add rust-src` and build `rust-analyzer` from sources, follow rust-analyzer [User Manual](https://rust-analyzer.github.io/manual.html#building-from-source).

For coc-rls do not add above configuration in `coc-settings.json` file just use ( `rustup component add rls rust-analysis rust-src`)

### SQL

Using [`sql-language-server`](https://github.com/joe-re/sql-language-server)

```jsonc
"languageserver": {
  "sql": {
    "command": "sql-language-server",
    "args" : ["up", "--method", "stdio"],
    "filetypes": ["sql", "mysql"]
    }
}
```

### Scala

Using [scalameta/metals](https://github.com/scalameta/metals):

Install [coc-metals](https://github.com/ckipp01/coc-metals), which will automate the Metals installation and also provide extra helpers.

If you'd like to use Metals without the coc-metals extension, make sure the generated metals-vim binary is available on your \$PATH and follow the instructions on the [Metals Website](http://scalameta.org/metals/docs/editors/vim.html).

```jsonc
"languageserver": {
  "metals": {
    "command": "metals-vim",
    "rootPatterns": ["build.sbt"],
    "filetypes": ["scala", "sbt"]
  }
}
```

Note that the Dotty Language server is no longer recommended. Instead, it is recommended to use Metals for Dotty/Scala 3.

### SystemVerilog

Using [svlangserver](https://github.com/imc-trading/svlangserver).

For installation, please check [this section in the readme](https://github.com/imc-trading/svlangserver#installation)

Example settings file
```jsonc
{
    "languageserver": {
        "svlangserver": {
            "module": "/INSTALLATION/PATH/lib/svlangserver.js",
            "filetypes": ["systemverilog"],
            "settings": {
                "systemverilog.includeIndexing": ["**/*.{sv,svh}"],
                "systemverilog.excludeIndexing": ["test/**/*.sv*"],
                "systemverilog.defines" : [],
                "systemverilog.launchConfiguration": "/TOOL/PATH/verilator -sv -Wall --lint-only",
                "systemverilog.formatCommand": "/TOOL/PATH/verible-verilog-format"
            }
        }
    }
}
```

### Terraform

Using [`Terraform-LSP`](https://github.com/juliosueiras/terraform-lsp). Build it and place it into any folder.

```jsonc
"languageserver": {
  "terraform": {
    "command": "terraform-lsp",
    "filetypes": ["terraform"],
    "initializationOptions": {}
  }
}
```

Using [`terraform-ls`](https://github.com/hashicorp/terraform-ls).

```jsonc
"languageserver": {
  "terraform": {
    "command": "terraform-ls",
    "args": ["serve"],
    "filetypes": ["terraform", "tf"],
    "initializationOptions": {}
  }
}
```

### Vala

Using [`vala-language-server`](https://github.com/benwaffle/vala-language-server)

```jsonc
"languageserver": {
  "vala": {
    "command": "vala-language-server",
    "filetypes": ["vala"],
  }
}
```

### Vue

Using [coc-vetur](https://github.com/neoclide/coc-vetur)

    :CocInstall coc-vetur

### Zig

Using [coc-zig](https://github.com/UltiRequiem/coc-zig)

    :CocInstall coc-zig



Using [`zls`](https://github.com/zigtools/zls#installation)

Make sure `zls` is findable in your `PATH` variable, otherwise specify the full path to the `zls` executable

```jsonc
"zig": {
  "command": "zls",
  "filetypes": [
    "zig"
  ]
},

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
      - '%f:%l %m'
      - '%f:%l:%c %m'
      - '%f: %l: %m'
```

coc-settings.json:

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

#### Using [Sorbet](https://sorbet.org):

```json
"languageserver": {
  "sorbet": {
    "command": "srb",
    "args": ["tc", "--typed", "true", "--enable-all-experimental-lsp-features", "--lsp", "--disable-watchman", "--dir", "."],
    "filetypes": ["ruby"],
    "rootPatterns": ["sorbet/config"],
    "initializationOptions": {},
    "settings": {}
  }
}
```

#### Using [Steep](https://github.com/soutaro/steep):

```json
"languageserver": {
  "steep": {
    "command": "steep",
    "args" : ["langserver"],
    "filetypes": ["ruby"]
  }
}
```
