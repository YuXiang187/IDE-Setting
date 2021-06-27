# Vim积累

```bash
syntax on
set number
set wrap
set showcmd
set wildmenu
set hlsearch
set incsearch
set smartcase
set nobackup
set nocompatible
set encoding=utf-8
set termencoding=utf-8
set fileencoding=utf-8
set fileencodings=ucs-bom,utf-8,cp936,gb18030
set imcmdline
set encoding=utf-8 fileencodings=ucs-bom,utf-8,cp936
```

## 0.乱七八糟

* `C`：调到单词尾、`B`调到单词首
* `cw`：更改这个词
* `ciw`：从单词中间删除单词
* `ci"`：删除引号内的全部文字
* `fv`：迅速让光标找紧接着光标下一个字母`v`
* `0`：回到开头
* `cf:`：一直删除到冒号处

## 1.解决Vim乱码

> 编辑Vim配置文件，默认位置如下：`C:\Program Files\Vim_vimrc`

将下面的代码加入到最后即可：

```bash
set encoding=utf-8
set termencoding=utf-8
set fileencoding=utf-8
set fileencodings=ucs-bom,utf-8,cp936,gb18030
set imcmdline
```

如果你想让Vim自动识别并转换编码：

```bash
set encoding=utf-8 fileencodings=ucs-bom,utf-8,cp936
```

解决consle输出乱码：`language messages zh_CN.utf-8`

Vim查看文件的编码方式：`set fileencoding`

* 以指定的编码打开某文件，如打开windows中以ANSI保存的文件：

  ```bash
  vim file.txt -c "e ++enc=GB18030"
  ```

* 文件编码转换，在Vim中直接进行转换文件编码，比如将一个文件转换成utf-8格式：

  ```bash
  :set fileencoding=utf-8
  ```

* 如果已经打开了解码错的文件，想重新设置编码格式：

  ```bash
  :edit ++enc=utf8
  ```

* 查看文件格式：

  ```bash
  :set fileformat?
  ```

* 设置文件格式为unix：

  ```bash
  :set fileformat=unix
  ```

重新加载Vim配置：

```bash
:source $MYVIMRC
```

## 2.Vim配置文件详解

> 更改键位：

```bash
`noremap a b # 将a键改成b键
```

> 更改键盘映射：

```bash
map s <nop> # 将s键设置为什么都不干
```

```bash
map S :w<CR> # 如果按下大写的S就保存文件
```

* 设置代码高亮：`syntax on`
* 开启行号：`set number`（关闭是`set nonumber `）
* 开启跟随光标行号：`set relativenumber`
* 在当前光标下显示一条线：`set cursorline`
* 自动换行：`set wrap`
* 在底部显示当前键入的指令：`set showcmd`
* 开启代码补全：`set wildmenu`
* 搜索时高亮显示：`set hlsearch`
* 搜索边输入边显示高亮：`set incsearch`
* 执行指令：`exec`，例如`exec nohlsearch`
* 忽略大小写：`set ignorecase`
* 检查大小写：`set smartcase`
* 主题：`color theme`
* 设定 tab 长度为4：`set tabstop=4`
* 覆盖文件时不备份：`set nobackup`
* 不与Vi兼容：`set nocompatible`
