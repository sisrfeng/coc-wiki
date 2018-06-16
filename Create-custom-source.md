Custom vim completion source could be created with simple steps.

## Start by a simple example

Assume `foo` as the source name.

* Create folder `autoload/coc/source` in any of your vim `runtimepath`, the default user runtimepath of neovim is `$XDG_CONFIG_HOME/nvim`

* Create a file name `foo.vim` in folder `autoload/coc/source`

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
That's all you need to get a basic custom completion source works.

## Default options of source

The options are returned by `coc#source#{name}#init` function, available options are:

* `shortcut`: used by menu, use source name if omitted.
* `priority`: a number for adjust sort results completion items, should be number of 0-99, default is `1`.
* `filetypes`: array of filetype names this source should be triggered, available for all filetypes if ommited.
* `firstMatch`: if not falsy, only the completion item that have first letter matched with user input would be shown.

all options are optional.

## Options for complete

``` typescript
id:               number // unqiue        number
bufnr:            number // current       buffer  number
line:             string // current       line    string
col:              number // the           start   column     number of input
input:            string // the           input   string
filetype:         string // filetype      of      current    buffer
filepath:         string // fullpath      of      current    buffer
changedtick:      number // b:changedtick when    start      completion
triggerCharacter: string // the           trigger character, could  be empty string of single character
colnr:            number // column        number  of         cursor 
linenr:           number // line          number  of         cursor
```

## Result of complete

## Optional functions