By use multiple cursors feature, you can change many places of current document at one time.

Multiple cursors are actually multiple text ranges.

The ranges are highlighted by `CocCursorRange` highlight group (linked to `Search` by default). To override the highlight, use command like:

``` vim
hi CocCursorRange guibg=#b16286 guifg=#ebdbb2
```
in your `.vimrc`

## Start multiple cursors session

There're some key-mappings for start multiple cursors:

* `<Plug>(coc-cursors-position)` add current character range to cursors.
* `<Plug>(coc-cursors-word)` add current word range to cursors.
* `<Plug>(coc-cursors-range)` add current visual selected range to cursors.
* `<Plug>(coc-cursors-operator)` use operator for add range to cursors.

Example usage:

``` viml
nmap <silent> <C-c> <Plug>(coc-cursors-position)
nmap <silent> <C-d> <Plug>(coc-cursors-word)
xmap <silent> <C-d> <Plug>(coc-cursors-range)
" use normal command like `<leader>xi(`
nmap <leader>x  <Plug>(coc-cursors-operator)
```

Or use internal command API to add ranges, like:
``` typescript
import {commands} from 'coc.nvim'
// hlRanges is a list of LSP Range
await commands.executeCommand('editor.action.addRanges', hlRanges)
```
Use vim script like:
``` viml
call CocAction('runCommand', 'editor.action.addRanges', hlRanges)
```

## Rename current variable

One common task is rename the variable name under cursor.

Use command `:CocCommand document.renameCurrentWord` to start cursors session with ranges contain current word.

**Note:** the word are extracted by `iskeyword` option of current buffer.

**Note:** it's simply strict match of words, instead of use rename feature of language server,

## Make changes for cursors

When cursors session is activated, you can change one placeholder to reflect changes for all cursor ranges.

* You can change the text any way you want to, like normal command or insert text.
* You can change around the text by command like `ysiw"` (with `tpope/vim-surround` installed).
* You can undo & redo the changes.
* You **can't** insert new line, it would cancel cursors session.

## Jump between ranges of cursors

* Use `"cursors.nextKey"` to jump to next position of cursors range, default to `<C-n>`.
* Use `"cursors.previousKey"` to jump to previous position of cursors range, default to `<C-p>`.

The key-mappings would be created when cursors session activated, and removed when session cancelled.

## Cancel cursors session

Use `"cursors.cancelKey"` to cancel cursors session, default to `<Esc>`.

The session would also be canceled when buffer change can't be applied for ranges or the cursor jump to another window.

## Use refactor action

Use `<Plug>(coc-refactor)` for create a refactor window of current symbol.

**Note:** it requires rename feature of language server to work.

When the window get opened, related ranges would be added to a new cursors session, you can:

* Change the placeholder for rename.
* Save the buffer for synchronize changes to related buffers.
* Add or remove lines.
* Press `<CR>` to open line under cursor in right split window.
* Change content of related buffers, the changes would be synchronized to refactor buffer.
* Fold the ranges by commands like `zi`.

<img width="632" alt="Screen Shot 2019-07-26 at 7 11 52 PM" src="https://user-images.githubusercontent.com/251450/61948104-8b385680-afd9-11e9-9858-cc980985ce3c.png">

Available configurations for refactor:

* `refactor.openCommand`: Open command for refactor window, default to `vsplit`.
* `refactor.beforeContext`: Print num lines of leading context before each match, default to `3`.
* `refactor.afterContext`: Print num lines of trailing context after each match., default to `3`.

## Use CocSearch command

For rename variable across files in current cwd, use `:CocSearch` command which requires [ripgrep](https://github.com/BurntSushi/ripgrep) to work.

Each range of lines would be added to refactor window asynchronously and matched ranges would be added to cursors session for rename.

<img width="620" alt="Screen Shot 2019-07-26 at 7 25 53 PM" src="https://user-images.githubusercontent.com/251450/61948853-8aa0bf80-afdb-11e9-8e81-107eaed53c4b.png">

Tips:

* Use `<tab>` to get available options after type `:CocSearch` command.
* Use `:CocSearch -w [word]` for word match.