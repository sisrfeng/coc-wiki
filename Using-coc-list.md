Coc list is created to make work with list of things easier, like locations.

Coc list is ispired by [denite.nvim](https://github.com/Shougo/denite.nvim), it's faster and doesn't require python support (mostly, you still need python available for vim8).

## List features

- **Insert mode and normal mode**, use insert mode for filter and normal mode to do everything else.
- **Actions for items**, each list item have different actions, you can create kepmapping for them and type `<tab>` to run one of them.
- **Multiple selection**, press `<space>` to toggle selection of an item or drag your mosue to select items.
- **Commands** for previous list:
   - `:CocListResume` reopen last list, restore window and cursor postion.
   - `:CocNext` do default action with next item.
   - `:CocPrev` do default action with previous item.
- **Different match modes**, coc use fuzzy match by default, but you can change to use strict match or regex match.
- **Interactive mode**, use `--interactive` in `:CocList` command to start list in interactive mode, when activated, all items would be fetched on input change, and the list was sorted and filtered by list implementation. 
  - Some source like `symbols` (use workspace symbols feature of language server) only works on interactive mode.
  - Interactive is only available when list support it.
- **Default key-mappings**, check out `:h coc-list-mappings` for default mappings, you can override them by use `"list.normalMappings"` and `"list.insertMappings"` in configuration file.
- **Auto preview feature:** the preview window would be adjusted when cursor moved in list window.
    ![2019-01-25 00_10_59](https://user-images.githubusercontent.com/251450/51693855-af22db80-203a-11e9-9bfe-a62cc49df23f.gif)

- **Number select feature**, add `--number-select` to `:CocList` command, then your can type 1-9 to do default action for that line of item.
- **Parse ansi from item label**, list can pass labels with ansi code, and the colors would be parsed and highlighted as expected

    <img width="747" alt="screen shot 2019-01-25 at 12 54 20 am" src="https://user-images.githubusercontent.com/251450/51694446-ca421b00-203b-11e9-9ee6-bd0259252f48.png">
    
    _Using highlight form output of ripgrep_

- **Mouse support**, you can click to change cursor position, use double click to select item and do default action, use drag to select multiple items.

- **Advanced fuzzy score**, for items with location, the match of filename would have higher score, beginning of path segment have higher score

   <img width="435" alt="screen shot 2019-01-25 at 1 06 16 am" src="https://user-images.githubusercontent.com/251450/51695213-759f9f80-203d-11e9-97bf-aeae5a09fdc5.png">

   _Only best match characters get highlighted_

- **Input history**, you can use `<C-n>` and `<C-p>` on insert mode to navigate command history list, the list is filtered with CWD and fuzzy match of current input (when not empty).

Checkout `:h coc-list` for detailed documentation.

## Tips

* When list is opened, you can still use mouse to scroll other vim windows.
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
* You can move to other window when list is opened, the prompt would be deactivated then.