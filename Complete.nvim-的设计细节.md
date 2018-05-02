因为发现现有的那些补全插件存在着一些问题，所以不得不弄个轮子来搞这个事情。

### 执行方式

开启补全后，并发执行所以补全任务，全部返回结果或者超时（300ms）时将所有结果合并后使用 feedkey 方式调用 `<Plug_>` 快捷键调用 `<C-R>= complete#show()<CR>` 方法，`complete#show` 调用 vim 提供的 `complete` 方法。
如果某个补全方法 300ms 没有返回，不进行中断处理，继续等待其返回值，如果 1s 后没有返回则对该方法进行中止（如果有提供 jobid）并忽略后续任何结果。

多个补全容许同时执行，使用 buffer hash + col + lnum 生成补全唯一 Id，不同补全 Id 拥有独立的补全列表

### 针对补全列表的缓存

如果任何一个补全方法失败（300ms 没有返回），不对结果进行缓存。
针对当前 buffer 不同补全 Id 缓存最近 10 次补全列表。
buffer 切换，dictionary 改变后缓存完全重置。
每个补全 Id 的补全列表 20s 后无使用则自动清除。

### 默认的补全类型

* buffer: 提取所有 buffer（不包括 nofile 和 terminal buffer）中的关键词进行补全
* dictionary: 提取所有当前 buffer 的 dictionary 文件中单词进行补全 

### Buffer 补全中的关键词处理

统一使用 `[\w_-$]`做为合法单词字符建立关键词列表。
如果单词长度大于 8 且包含 `_` `-` 且补全发生在当前文件，给出分割后除最后一个单词做为可用关键词，这样做可以更方便编写较长名字的 css class

### Dictionary 补全中的特殊处理

* 建立 buffer dictionary 映射，Buffer 进入时读取文件并载入，buffer 退出时如果没有 buffer 使用则清理掉该 dictionary 缓存。
* 监听 dictionary 改变事件，改变时即时调整缓存

### 针对带补全单词的不同处理

Buffer 和  Dictionary 提供了单词补全，这里要考虑已有的单词情况进行不同处理。

* 如果单词长度为 0，则不进行处理
* 如果单词长度为 1，则必须第一个字母匹配
* 如果单词长度大于 1，则进行 fuzzy match，针对头部（部分符号，例如 `-` `_` 之后或者驼峰上的字母）命中给与优先加成

### 针对路径补全的处理逻辑

路径补全应该只在特定条件下生效，例如 javascript 文件中当前行有 import， require 且单词处于字符串中。
针对不个类型文件提供 vim 方法来判定是否需要使用路径补全。
路径补全是排它的，生效时其它补全不可用
路径补全时从 CWD 进行文件查找，补全后插入文件相对路径
路径补全查找时使用路径专用算法排序：例如 fzy 的算法

### 错误处理

针对后台报错使用 log4js 保存在 `$TMP/complete.log` 中，使用 fundebug 进行上报（用户没禁止的话）。
需要提示用户的错误单行使用 `echoerr`，多行使用 `complete#util#show_error` 方法打开下方临时窗口 

### 针对 LSP 中定义的 `text_edit` 以及 `snippet` 类型的处理

使用 complete-item 中的 user-data 字段以及 `completeDone` 事件来完成， user-data 中为 json 字符串，可包含 `text_edit` `is_snippet` 字段

### 自定义补全扩展

* `complete#source#{name}#init()` 返回 source 配置信息，如 `name` `shortcut` `priority` `filetypes` `engross`， 除了 `name` 都是可选的
   * name： 唯一名称标识
   * shortcut： 长度小于3的字符，标识 complete item 的类型
   * priority： 优先度数值，越大则越优先
   * filetypes： 可作用的 filetype 列表，不提供则表示所有文件可用
   * engross：独占模式，为 1 时运行时忽略所有其它补全，忽略 priority，必须提供 should_complete 方法
* `complete#source#{name}#should_complete(opt)` 返回 1 或者 0 表示当前状态是否要运行补全，不提供该函数表示总是补全
   * `opt.line` 当前行字符串
   * `opt.start` 补全其实字符
   * `opt.input` 用户已输入的待补全部分
   * `opt.id` 补全 id 标识符
* `complete#source#{name}#complete(opt, callback)` 提供执行补全的逻辑实现，调用 `callback` 传递结果，可返回 jobstart 返回的 channel id
  *  `opt` 参数与 should_complete 相同
  *  callback 函数可以传递一个 result 对象:
    * items： 必须是 complete-items 数组，查看 `:h complete-items`
    * offset-left: 必须正整数，表示完成后清除掉补全起始位置之前的若干个字符 （使用时 word 不允许换行）
    * offset-right: 必须正整数，表示完成后清除掉补全终止位置之后的若干个字符 （使用时 word 不允许换行）

