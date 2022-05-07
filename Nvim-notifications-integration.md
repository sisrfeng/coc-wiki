## Integration with [nvim-notify](https://github.com/rcarriga/nvim-notify)

### Status and Diagnostics

<img width="948" alt="coc" src="https://user-images.githubusercontent.com/18579817/166508592-cc96612f-ca40-409d-9615-7880c790f5ad.png">

Add the following snippet to your `init.vim`, after `notify` extension is loaded and `vim.notify` is pointing to `require("notify")`.

```vim
lua << EOF

local coc_status_record = {}

function coc_status_notify(msg, level)
  local notify_opts = { title = "LSP Status", timeout = 500, hide_from_history = true, on_close = reset_coc_status_record }
  -- if coc_status_record is not {} then add it to notify_opts to key called "replace"
  if coc_status_record ~= {} then
    notify_opts["replace"] = coc_status_record.id
  end
  coc_status_record = vim.notify(msg, level, notify_opts)
end

function reset_coc_status_record(window)
  coc_status_record = {}
end

local coc_diag_record = {}

function coc_diag_notify(msg, level)
  local notify_opts = { title = "LSP Diagnostics", timeout = 500, on_close = reset_coc_diag_record }
  -- if coc_diag_record is not {} then add it to notify_opts to key called "replace"
  if coc_diag_record ~= {} then
    notify_opts["replace"] = coc_diag_record.id
  end
  coc_diag_record = vim.notify(msg, level, notify_opts)
end

function reset_coc_diag_record(window)
  coc_diag_record = {}
end
EOF

function! s:DiagnosticNotify() abort
  let l:info = get(b:, 'coc_diagnostic_info', {})
  if empty(l:info) | return '' | endif
  let l:msgs = []
  let l:level = 'info'
   if get(l:info, 'warning', 0)
    let l:level = 'warn'
  endif
  if get(l:info, 'error', 0)
    let l:level = 'error'
  endif
 
  if get(l:info, 'error', 0)
    call add(l:msgs, ' Errors: ' . l:info['error'])
  endif
  if get(l:info, 'warning', 0)
    call add(l:msgs, ' Warnings: ' . l:info['warning'])
  endif
  if get(l:info, 'information', 0)
    call add(l:msgs, ' Infos: ' . l:info['information'])
  endif
  if get(l:info, 'hint', 0)
    call add(l:msgs, ' Hints: ' . l:info['hint'])
  endif
  let l:msg = join(l:msgs, "\n")
  if empty(l:msg) | let l:msg = ' All OK' | endif
  call v:lua.coc_diag_notify(l:msg, l:level)
endfunction

function! s:StatusNotify() abort
  let l:status = get(g:, 'coc_status', '')
  let l:level = 'info'
  if empty(l:status) | return '' | endif
  call v:lua.coc_status_notify(l:status, l:level)
endfunction

function! s:InitCoc() abort
  execute "lua vim.notify('Initialized coc.nvim for LSP support', 'info', { title = 'LSP Status' })"
endfunction

" notifications
autocmd User CocNvimInit call s:InitCoc()
autocmd User CocDiagnosticChange call s:DiagnosticNotify()
autocmd User CocStatusChange call s:StatusNotify()
```

### Intercepting messages

<img width="573" alt="coc-messages" src="https://user-images.githubusercontent.com/18579817/166982575-e3c6bbc4-7cbc-474c-8ea7-77e17ed62c3e.png">

`coc.nvim` uses echo* commands to write variety of messages. In order to intercept those, monkey patching it by utilizing vim's `after` directory (see `:help after-directory`) is one approach. 

First, create `coc/util.vim` file in the `autoload` after-directory in your vim's runtime path (e.g. `~/.vim/after/autoload`). For instance, place the following content in the file `~/.vim/after/autoload/coc/util.vim` --

```vim
scriptencoding utf-8

if has("nvim")
  " overwrite coc#util#echo_messages function to use notify
  function! coc#util#echo_messages(hl, msgs)
    if a:hl !~# 'Error' && (mode() !~# '\v^(i|n)$')
      return
    endif
    let msgs = filter(copy(a:msgs), '!empty(v:val)')
    if empty(msgs)
      return
    endif
    " map a:hl highlight groups to notify levels
    " if hl matches Error then level is error
    " if hl matches Warning then level is warn
    " otherwise level is info
    let level = 'info'
    if a:hl =~# 'Error'
      let level = 'error'
    elseif a:hl =~# 'Warning'
      let level = 'warn'
    endif
    let msg = join(msgs, '\n')
    call v:lua.coc_notify(msg, level)
  endfunction

  function! coc#util#echo_lines(lines)
    let msg = join(a:lines, "\n")
    call v:lua.coc_notify(msg, 'info')
  endfunction
endif


```

Next, manually source this file after coc.nvim has initialized. You can modify `s:InitCoc` function from the previous example/section and add the function `coc_notify` --

```vim
function! s:InitCoc() abort
  " load overrides
  runtime! autoload/coc/util.vim
  execute "lua vim.notify('Initialized coc.nvim for LSP support', 'info', { title = 'LSP Status' })"
endfunction

function coc_notify(msg, level)
  local notify_opts = { title = "LSP Message", timeout = 500 }
  vim.notify(msg, level, notify_opts)
end
```