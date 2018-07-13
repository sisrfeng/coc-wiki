# Contents

* [Supported features](https://github.com/neoclide/coc.nvim/wiki/Language-servers#supported-features)
* [Built in server extensions](https://github.com/neoclide/coc.nvim/wiki/Language-servers#built-in-server-extensions)
* [Register custom language servers](https://github.com/neoclide/coc.nvim/wiki/Language-servers#regist-custom-language-servers)

## Supported features

Check out the official specification at https://microsoft.github.io/language-server-protocol/specification.

* ✓ Request cancellation support
* ✓ Full features of workspace (except workspace folders related)
* ✓ Full features of text synchronization
* ✓ Full features of window support
* ✓ Diagnostics
* ✗ Telemetry
* ✓ Completion
* ✓ Completion resolve
* ✓ Hover
* ✓ Signature help
* ✓ Definition
* ✓ Type definition
* ✓ Implementation
* ✓ References
* ✓ Document highlight
* ✓ Document symbol
* ✓ Code action
* ✓ CodeLens
* ✓ CodeLens resolve
* ✗ Document link
* ✗ Document link resolve
* ✗ Document color
* ✗ Color Presentation
* ✓ Document Formatting
* ✓ Document Range Formatting
* ✗ Document on Type Formatting
* ✓ Rename

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
solargraph   | ruby                    | [vscode-solargraph](https://github.com/castwide/vscode-solargraph)
stylelint    | css, wxss, scss...      | [vscode-stylelint](https://github.com/shinnn/vscode-stylelint)
tslint       | typescript, javascript  | [vscode-tslint](https://github.com/Microsoft/vscode-tslint)
wxml         | wxml                    | [wxml-langserver](https://github.com/chemzqm/wxml-languageserver)

For settings of built in extensions, check out [Extension configuration](https://github.com/neoclide/coc.nvim/wiki/Using-configuration-file#extension-configuration)

## Register custom language servers

