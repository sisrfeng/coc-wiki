# Contents

## Why yet another completion engine?

- 🚀 **Fast**: [instant increment completion](https://github.com/neoclide/coc.nvim/wiki/Completion-with-sources#highlights-of-coc-completion), increment buffer sync using buffer update events.
- 💎 **Reliable**: typed language, tested with CI.
- 🌟 **Featured**: [full LSP support](https://github.com/neoclide/coc.nvim/wiki/Language-servers#supported-features)
- ❤️  **Flexible**: [configured like VSCode](https://github.com/neoclide/coc.nvim/wiki/Using-the-configuration-file), [extensions work like in VSCode](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions)

<details><summary>Completion experience</summary>
Below are the reasons that led coc.nvim to build its own engine:

- **Full LSP completion support**, especially snippet and `additionalTextEdit` feature, you'll understand why it's awesome when you experience it with a coc extension like `coc-tsserver`.
- **Asynchronous and parallel completion request**, unless using vim sources, your vim will never be blocked.
- **Does completion resolving on completion item change**. The details from completion items are echoed after being selected, this feature requires the `CompleteChanged` autocmd to work.
- **Incomplete request and cancel request support**, only incomplete completion requests would be triggered on filtering completion items and cancellation requests are sent to servers only when necessary.
- **Start completion without timer**. The completion will start after you type the first letter of a word by default and is filtered with new input after the completion has finished. Other completion engines use a timer to trigger completion so you always have to wait after the typed character.
- **Realtime buffer keywords**. Coc will generate buffer keywords on buffer change in the background (with debounce), while some completion engines use a cache which isn't always correct.  Plus, [Locality bonus feature](https://code.visualstudio.com/docs/editor/intellisense#_locality-bonus) from VSCode is enabled by default.
- **Filter completion items when possible.** When you do a fuzzy filter with completion items, some completion engines will trigger a new completion, but coc.nvim will filter the items when possible which makes it much faster. Filtering completion items on backspace is also supported.
</details>


## [Install coc.nvim](https://github.com/neoclide/coc.nvim/wiki/Install-coc.nvim)

* [Requirements](https://github.com/neoclide/coc.nvim/wiki/Install-coc.nvim#requirements)
* [Add coc.nvim to your vim's runtimepath](https://github.com/neoclide/coc.nvim/wiki/Install-coc.nvim#add-cocnvim-to-your-vims-runtimepath)
* [Automation script](https://github.com/neoclide/coc.nvim/wiki/Install-coc.nvim#automation-script)

## [Completion with sources](https://github.com/neoclide/coc.nvim/wiki/Completion-with-sources)

* [Trigger mode of completion](https://github.com/neoclide/coc.nvim/wiki/Completion-with-sources#trigger-mode-of-completion)
* [Use `<Tab>` or custom key for trigger completion](https://github.com/neoclide/coc.nvim/wiki/Completion-with-sources#use-tab-or-custom-key-for-trigger-completion)
* [Improve completion experience](https://github.com/neoclide/coc.nvim/wiki/Completion-with-sources#improve-completion-experience)
* [Completion sources](https://github.com/neoclide/coc.nvim/wiki/Completion-with-sources#completion-sources)

## [Using the configuration file](https://github.com/neoclide/coc.nvim/wiki/Using-the-configuration-file)

* [Configuration file resolve](https://github.com/neoclide/coc.nvim/wiki/Using-the-configuration-file#configuration-file-resolve)
* [Default COC preferences](https://github.com/neoclide/coc.nvim/wiki/Using-the-configuration-file#default-coc-preferences)
* [Configuration for sources](https://github.com/neoclide/coc.nvim/wiki/Using-the-configuration-file#configuration-for-sources)
* [Extension configuration](https://github.com/neoclide/coc.nvim/wiki/Using-the-configuration-file#extension-configuration)

## [Using coc extensions](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions)

* [Why are coc extensions needed?](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions#why-are-coc-extensions-needed)
* [Manage coc extensions](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions#manage-coc-extensions)
  * [Install extensions](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions#install-extensions)
  * [Use vim's plugin manager for coc extension](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions#use-vims-plugin-manager-for-coc-extension)
* [Implemented coc extensions](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions#implemented-coc-extensions)
* [Debug coc extension](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions#debug-coc-extension)

## [Language servers](https://github.com/neoclide/coc.nvim/wiki/Language-servers)

* [Supported features](https://github.com/neoclide/coc.nvim/wiki/Language-servers#supported-features)
* [Register custom language servers](https://github.com/neoclide/coc.nvim/wiki/Language-servers#register-custom-language-servers)
* [Debug language server](https://github.com/neoclide/coc.nvim/wiki/Debug-language-server)

## [Create custom source](https://github.com/neoclide/coc.nvim/wiki/Create-custom-source)

* [Start by a simple example](https://github.com/neoclide/coc.nvim/wiki/Create-custom-source#start-by-a-simple-example)
* [Default options of source](https://github.com/neoclide/coc.nvim/wiki/Create-custom-source#default-options-of-source)
* [Options for complete](https://github.com/neoclide/coc.nvim/wiki/Create-custom-source#options-for-complete)
* [Result of complete](https://github.com/neoclide/coc.nvim/wiki/Create-custom-source#result-of-complete)
* [Optional functions](https://github.com/neoclide/coc.nvim/wiki/Create-custom-source#optional-functions)

## [Environment variables](https://github.com/neoclide/coc.nvim/wiki/Environment-variables)

* [Logger](https://github.com/neoclide/coc.nvim/wiki/Environment-variables#logger)

## [F.A.Q](https://github.com/neoclide/coc.nvim/wiki/F.A.Q)

* [Why does the `omni` source require user configuration to work?](https://github.com/neoclide/coc.nvim/wiki/F.A.Q#why-omni-source-requires-user-configuration-to-work)
* [How could separate `ultisnips` source from COC?](https://github.com/neoclide/coc.nvim/wiki/F.A.Q#how-could-separate-ultisnips-source-from-coc)
* [Why does `omni` not work even if enabled in configuration?](https://github.com/neoclide/coc.nvim/wiki/F.A.Q#why-omni-doesnt-work-even-if-enabled-in-configuration)
* [Is it possible to highlight the characters in complete items?](https://github.com/neoclide/coc.nvim/wiki/F.A.Q#is-it-possible-to-highlight-the-characters-in-complete-items)
* [How to change highlight of diagnostic signs?](https://github.com/neoclide/coc.nvim/wiki/F.A.Q#how-to-change-highlight-of-diagnostic-signs)

## Similar projects

* [YouCompleteMe](https://github.com/Valloric/YouCompleteMe)
* [deoplete.nvim](https://github.com/Shougo/deoplete.nvim)
* [asyncomplete.vim](https://github.com/prabirshrestha/asyncomplete.vim)
* [vim-mucomplete](https://github.com/lifepillar/vim-mucomplete/)
* [ncm2](https://github.com/ncm2/ncm2)
* [completor.vim](https://github.com/maralla/completor.vim)

