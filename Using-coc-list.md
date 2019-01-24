Coc list is created to make work with list of objects easier, like locations.

Coc list is ispired by [denite.nvim](https://github.com/Shougo/denite.nvim), it's faster and doesn't require python support (mostly, you still need python available for vim8).

## List features

- Insert mode and normal mode, use insert mode for filter and normal mode to do everything else.
- Actions for items, each list item have different actions, you can create kepmapping for them and type `<tab>` to run one of them.
- Multiple selection, press `<space>` to toggle selection of an item or drag your mosue to select items.
- Commands for previous list:
   - `:CocListResume` reopen last list, restore window and cursor postion.
   - `:CocNext` do default action with next item.
   - `:CocPrev` do default action with previous item.
- Different match mode, coc use fuzzy match by default, but you can change to use strict match or regex match.
- Interactive mode, use `--interactive` in `:CocList` command to start list in interactive mode, when activated, all items would be fetched on input change, and the list was sorted and filtered by list implementation. 
  - Some source like `symbols` (use workspace symbols feature of language server) only works on interactive mode.
- Default keymappings, check out `:h coc-list-mappings` for default mappings, you can override them by use `"list.normalMappings"` and `"list.insertMappings"` in configuration file.
- Auto preview feature: the preview window would be adjusted when cursor moved in list window.
    ![2019-01-25 00_10_59](https://user-images.githubusercontent.com/251450/51693855-af22db80-203a-11e9-9bfe-a62cc49df23f.gif)



Checkout `:h coc-list` for detailed documentation.