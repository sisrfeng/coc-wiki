Coc list is created to make working with lists of things easier, ex: locations & extensions.

Coc list is inspired by [denite.nvim](https://github.com/Shougo/denite.nvim). It's faster.

## List features

- **Insert mode and normal mode**, use insert mode for filter and normal mode to do everything else.
- **Actions for items**, each list provides different actions. You can create key-mappings for them and type `<tab>` to run one of them.
- **Multiple selection**, you can do multiple selection in different ways:
  - Press `<space>` to toggle selection of an item.
  - Drag your mouse to select items.
  - On normal mode, select lines in visual select mode and then press `<space>` to toggle selection of lines.
- **Commands** for previous list:
   - `:CocListResume` reopen last list, restore window and cursor position.
   - `:CocNext` do default action with next item.
   - `:CocPrev` do default action with previous item.
- **Different match modes**, coc use fuzzy match by default, but you can change to use strict match or regex match by `<c-s>`.
- **Interactive mode**, use `--interactive` in `:CocList` command to start list in interactive mode, when activated, all items would be fetched on input change, and the list was sorted and filtered by list implementation. 
  - Some source like `symbols` (use workspace symbols feature of language server) only works on interactive mode.
  - Interactive is only available when the list supports it.
- **Default key-mappings**, check out `:h coc-list-mappings` for default mappings, you can override them by use `"list.normalMappings"` and `"list.insertMappings"` in configuration file.
- **Auto preview feature:** the preview window would be adjusted when cursor moved in list window.
    ![2019-01-25 00_10_59](https://user-images.githubusercontent.com/251450/51693855-af22db80-203a-11e9-9bfe-a62cc49df23f.gif)

- **Number select feature**, add `--number-select` to `:CocList` command, then your can type 1-9 to do default action for that line of item.
- **Parse ansi from item label**, list can pass labels with ansi code, and the colors would be parsed and highlighted as expected

    <img width="747" alt="screen shot 2019-01-25 at 12 54 20 am" src="https://user-images.githubusercontent.com/251450/51694446-ca421b00-203b-11e9-9ee6-bd0259252f48.png">
    
    _Using highlight form output of ripgrep_

- **Mouse support**, you can click to change cursor position, use double click to select item and do default action, and use drag to select multiple items.

- **Advanced fuzzy score**, for items with location, the match of filename will have a higher score, beginning of path segment have higher score

   <img width="435" alt="screen shot 2019-01-25 at 1 06 16 am" src="https://user-images.githubusercontent.com/251450/51695213-759f9f80-203d-11e9-97bf-aeae5a09fdc5.png">

   _Only best match characters get highlighted_

- **Input history**, you can use `<C-n>` and `<C-p>` on insert mode to navigate command history list, the list is filtered with CWD and fuzzy match of current input (when not empty).

Checkout `:h coc-list` for detailed documentation.

## Tips

* When list is open, you can still use mouse to scroll other vim windows.
* Create custom key-mappings for do action easier, for example:

    ``` json
    "list.normalMappings": {
      "t": "action:tabe",
      "v": "action:vsplit",
      "s": "action:split",
      "d": "expr:GetDeleteAction"
    },
    "list.insertMappings": {
      "<C-t>": "action:tabe",
      "<C-w>": "command:wincmd k"
    },
    ```
* You can move to another window when the list is opened, the prompt would be deactivated then.
* Use variables `g:terminal_color_{0-7}` to customize ansi highlight colors, same as terminal colors of neovim `:h terminal-configuration`.
* Press `?` on normal mode to get help.
  <img width="1158" alt="screen shot 2019-02-04 at 8 16 29 am" src="https://user-images.githubusercontent.com/251450/52185008-731b2200-2855-11e9-8bea-9068d016e8c9.png">

## Builtin list sources

- `outline` outline of current document, provided by language server or `ctags`.
- `sources` loaded completion sources.
- `symbols` search workspace symbol.
- `commands` registered coc commands.
- `location` latest jump locations.
- `services` registered language client services.
- `extensions` installed extensions.
- `diagnostics` diagnostics of current workspace
- `links` links of current document, provided by language server.
- `lists` available list sources.
- `output` current output channels.

## Extensions

Some coc extensions make use of list feature:

- [coc-lists](https://github.com/neoclide/coc-lists) provide some basic lists, including `files`, `mru`, `grep` etc.
- [coc-snippets](https://github.com/neoclide/coc-snippets) provide snippets list.
