A custom vim completion source can be created with a few simple steps.

# Contents

* [Start with a simple example](#start-with-a-simple-example)
* [Default options of source](#default-options-of-source)
* [Options for completion](#options-for-completion)
* [Results of completion](#result-of-completion)
* [Optional functions](#optional-functions)

## Start with a simple example

Assume `foo` is the source name.

* Create folder `autoload/coc/source` in any of your vim `runtimepath`, the default user runtimepath of neovim is `$XDG_CONFIG_HOME/nvim`

* Create a file named `foo.vim` in folder `autoload/coc/source`

* Add content:
    ``` vim
    function! coc#source#foo#init() abort
      " options of current source
      return {
            \'shortcut': 'foo',
            \'priority': 3,
            \}
    endfunction

    function! coc#source#foo#complete(opt, cb) abort
      let items = [{
            \ "word": "foo"
            \}, {
            \ "word": "bar"
            \}]
      call a:cb(items)
    endfunction
    ```
That's all you need to do in order to get a basic custom completion source working.

## Default options of source

The options are returned by `coc#source#{name}#init` function, available options are:

* `shortcut`: used by menu, uses source name if omitted.
* `priority`: a number for adjusting the sort results completion items. It should be number between 0-99. The  default is `1`.
* `filetypes`: array of filetype names this source should be triggered by. Available for all filetypes if ommited.
* `firstMatch`: if not falsy, only the completion item that has the first letter matching the user input will be shown.

All options are optional.

## Options for completion

``` typescript
  id              : number // unqiue number
  bufnr           : number // current buffer number
  line            : string // current line string
  col             : number // the start column number of input
  input           : string // the input string
  filetype        : string // filetype of current buffer
  filepath        : string // fullpath of current buffer
  changedtick     : number // b:changedtick when start completion
  triggerCharacter: string // the trigger character, could be empty string of single character
  colnr           : number // column number of cursor 
  linenr          : number // line number of cursor
```

## Result of completion

Result are returned by a callback function (second argument). The result should be list of vim completion items, or fasly value for invalid results.

Check out `:h complete-items` for available fields.

## Optional functions

* `coc#source#{name}#refresh()`: called when user does a refresh action for source.
* `coc#source#{name}#should_complete(option)`: called with completion option, returns 0 or other fasly value to skip completion for current completion.
* `coc#source#{name}#get_startcol(option)`: called with completion option, should return completion start column number when function exists, other source is disabled if the returned column number is not `option.col`
* `coc#source#{name}#on_complete(item)`: called with vim complete item when it's selected by user.