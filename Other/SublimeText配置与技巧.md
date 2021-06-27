# SublimeText配置与技巧

## 0.技巧

* **垂直选区**：按住<kbd>鼠标中键</kbd>kbd>然后滑动鼠标即可垂直选区（<kbd>Shift</kbd>+<kbd>鼠标右键</kbd>和<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>↓</kbd>也可以）
* **快速选中**：选中文本后按住<kbd>Alt</kbd>+<kbd>F3</kbd>快速选择指定字符
* **自适应缩进的粘贴：**使用<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>V</kbd>

## 1.设置

请点击菜单“Preferences - Settings”

```css
{
	"color_scheme": "Packages/Color Scheme - Default/Monokai.sublime-color-scheme",
	"font_face": "Consolas",
	"font_size": 13,
    // 设置闪烁的光标样式
    "caret_style": "smooth",
	"ignored_packages":
	[
		"Vintage"
	],
	"update_check": false,
    // The number of spaces a tab is considered equal to
    "tab_size": 4,
    // Set to true to insert spaces when tab is pressed
    "translate_tabs_to_spaces": true,
    //设置保存时自动转换
    "expand_tabs_on_save": true
}
```

## 2.文件模板

你必须安装sublimeTepl

请点击菜单“Preferences - Package Settings - SublimeTmpl - Settings - User”，下面是HTML模板的一个例子：

```css
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
            <title>main title</title>
            <meta name="keywords" content="search word">
            <meta name="description" content="introduction">
            <link rel="stylesheet" type="text/css" href="css/style.css">
            <script type="" src="js/jquery-2.1.4.js" charset="UTF-8"></script>
    </head>
    <body>
    </body>
</html>
```

对了，相应的模板在`C:\Users\ usersName\AppData\Roaming\Sublime Text 3\Packages\SublimeTmpl\templates`下

* Ctrl+Alt+H：新建 html 文件
* Ctrl+Alt+J： 新建 javascript 文件
* Ctrl+Alt+C： 新建 css 文件
* Ctrl+Alt+P：新建 php 文件
* Ctrl+Alt+R： 新建 ruby 文件
* Ctrl+Alt+Shift+P：新建 python 文件

## 3.ConvertToUTF8 插件

ConvertToUTF8 能将除UTF8编码之外的其他编码文件在 Sublime Text 中转换成UTF8编码

## 4.BracketHighlighter 插件

高亮显示匹配的括号、引号和标签

## 5.ColorHighlighter 插件

显示所选颜色值的颜色，并集成了ColorPicker

在16进制的颜色值上点右键，选择“Choose color”,会弹出颜色拾色器，在需要的色块上单击。

## 6.AutoPep8 插件

自动将 Python 代码按 PEP8 规范格式化，安装完添加如下配置可自动在保存文件的时候格式化：

```css
{
	"format_on_save": true,
}
```

## 7.AutoFileName 插件

自动补全文件(目录)名

## 8.SideBarEnhancements 插件

侧栏菜单扩充功能

## 9.Anaconda 插件

代码分析平台，用于代码规范检查

**警告：禁止和Jedi共存，否则输入不了左括号**

## 10.Jedi 插件

Python智能补全插件

**警告：禁止和Anaconda共存，否则输入不了左括号**

## 11.Theme-Soda 主题

Sublime Text 最受欢迎的主题之一，风格类似GNOME桌面

单击菜单栏"Preferences" - "Package Control"，输入`Install Package`，然后输入`Theme-Soda`即可安装主题

* Soda 亮色主题请添加：

  ```css
  {
      "soda_classic_tabs": true,
      "theme": "Soda Light 3.sublime-theme",
  }
  ```

* Soda 暗色主题请添加：

  ```css
  {
      "soda_classic_tabs": true,
      "theme": "Soda Dark 3.sublime-theme",
  }
  ```

## 方法：Sublime text输入括号后快速跳出

将下列代码绑定到"Preferences" - "KeyBindings"

```python
[
    {"keys": ["enter"], "command": "move", "args": {"by": "characters", "forward": true}, "context":
        [
            { "key": "following_text", "operator": "regex_contains", "operand": "^[)\\]\\>\\'\\\"\\ %>\\}\\;\\,]", "match_all": true },
            { "key": "preceding_text", "operator": "not_regex_match", "operand": "^.*\\{$", "match_all": true  },
            { "key": "auto_complete_visible", "operator": "equal", "operand": false }
        ]
    }
]
```

保存后在文本区域内输入一个括号，按下<kbd>Enter</kbd>试试

## 附：Sublime Text3 全部快捷键

### 命令类

```
Ctrl+Shift+P：打开命令面板
Ctrl+P：搜索项目中的文件
Ctrl+G：跳转到第几行
Ctrl+W：关闭当前打开文件
Ctrl+Shift+W：关闭所有打开文件
Ctrl+Shift+V：粘贴并格式化
Ctrl+D：选择单词，重复可增加选择下一个相同的单词
Ctrl+L：选择行，重复可依次增加选择下一行
Ctrl+Shift+L：选择多行
Ctrl+Shift+Enter：在当前行前插入新行
Ctrl+X：删除当前行
Ctrl+M：跳转到对应括号
Ctrl+U：软撤销，撤销光标位置
Ctrl+J：选择标签内容
Ctrl+F：查找内容
Ctrl+Shift+F：查找并替换
Ctrl+H：替换
Ctrl+R：前往 method
Ctrl+N：新建窗口
Ctrl+K+B：开关侧栏
Ctrl+Shift+M：选中当前括号内容，重复可选着括号本身
Ctrl+F2：设置/删除标记
Ctrl+/：注释当前行
Ctrl+Shift+/：当前位置插入注释
Ctrl+Alt+/：块注释，并Focus到首行，写注释说明用的
Ctrl+Shift+A：选择当前标签前后，修改标签用的
F11：全屏
Shift+F11：全屏免打扰模式，只编辑当前文件
Alt+F3：选择所有相同的词
Alt+.：闭合标签
Alt+Shift+数字：分屏显示
Alt+数字：切换打开第N个文件
Shift+右键拖动：光标多不，用来更改或插入列内容
鼠标的前进后退键可切换Tab文件
按Ctrl，依次点击或选取，可需要编辑的多个位置
按Ctrl+Shift+上下键，可替换行
```

### 选择类

```
Ctrl+D 选中光标所占的文本，继续操作则会选中下一个相同的文本。
Alt+F3 选中文本按下快捷键，即可一次性选择全部的相同文本进行同时编辑。举个栗子：快速选中并更改所有相同的变量名、函数名等。
Ctrl+L 选中整行，继续操作则继续选择下一行，效果和 Shift+↓ 效果一样。
Ctrl+Shift+L 先选中多行，再按下快捷键，会在每行行尾插入光标，即可同时编辑这些行。
Ctrl+Shift+M 选择括号内的内容（继续选择父括号）。举个栗子：快速选中删除函数中的代码，重写函数体代码或重写括号内里的内容。
Ctrl+M 光标移动至括号内结束或开始的位置。
Ctrl+Enter 在下一行插入新行。举个栗子：即使光标不在行尾，也能快速向下插入一行。
Ctrl+Shift+Enter 在上一行插入新行。举个栗子：即使光标不在行首，也能快速向上插入一行。
Ctrl+Shift+[ 选中代码，按下快捷键，折叠代码。
Ctrl+Shift+] 选中代码，按下快捷键，展开代码。
Ctrl+K+0 展开所有折叠代码。
Ctrl+← 向左单位性地移动光标，快速移动光标。
Ctrl+→ 向右单位性地移动光标，快速移动光标。
shift+↑ 向上选中多行。
shift+↓ 向下选中多行。
Shift+← 向左选中文本。
Shift+→ 向右选中文本。
Ctrl+Shift+← 向左单位性地选中文本。
Ctrl+Shift+→ 向右单位性地选中文本。
Ctrl+Shift+↑ 将光标所在行和上一行代码互换（将光标所在行插入到上一行之前）。
Ctrl+Shift+↓ 将光标所在行和下一行代码互换（将光标所在行插入到下一行之后）。
Ctrl+Alt+↑ 向上添加多行光标，可同时编辑多行。
Ctrl+Alt+↓ 向下添加多行光标，可同时编辑多行。
```

### 编辑类

```
Ctrl+J 合并选中的多行代码为一行。举个栗子：将多行格式的CSS属性合并为一行。
Ctrl+Shift+D 复制光标所在整行，插入到下一行
Tab 向右缩进。
Shift+Tab 向左缩进
Ctrl+K+K 从光标处开始删除代码至行尾
Ctrl+Shift+K 删除整行
Ctrl+/ 注释单行。
Ctrl+Shift+/ 注释多行
Ctrl+K+U 转换大写
Ctrl+K+L 转换小写
Ctrl+Z 撤销
Ctrl+Y 恢复撤销
Ctrl+U 软撤销，感觉和 Ctrl+Z 一样
Ctrl+F2 设置书签
Ctrl+T 左右字母互换
F6 单词检测拼写
```

### 搜索类

```
Ctrl+F 打开底部搜索框，查找关键字。
Ctrl+shift+F 在文件夹内查找，与普通编辑器不同的地方是sublime允许添加多个文件夹进行查找，略高端，未研究。
Ctrl+P 打开搜索框。例如：1、输入当前项目中的文件名，快速搜索文件，2、输入@和关键字，查找文件中函数名，3、输入：和数字，跳转到文件中该行代码，4、输入#和关键字，查找变量名。
Ctrl+G 打开搜索框，自动带：，输入数字跳转到该行代码。举个栗子：在页面代码比较长的文件中快速定位。
Ctrl+R 打开搜索框，自动带@，输入关键字，查找文件中的函数名。举个栗子：在函数较多的页面快速查找某个函数。
Ctrl+： 打开搜索框，自动带#，输入关键字，查找文件中的变量名、属性名等。
Ctrl+Shift+P 打开命令框。场景栗子：打开命名框，输入关键字，调用sublime text或插件的功能，例如使用package安装插件。
Esc 退出光标多行选择，退出搜索框，命令框等。
```

### 显示类

```
Ctrl+Tab 按文件浏览过的顺序，切换当前窗口的标签页
Ctrl+PageDown 向左切换当前窗口的标签页
Ctrl+PageUp 向右切换当前窗口的标签页
Alt+Shift+1 窗口分屏，恢复默认1屏（非小键盘的数字）
Alt+Shift+2 左右分屏-2列
Alt+Shift+3 左右分屏-3列
Alt+Shift+4 左右分屏-4列
Alt+Shift+5 等分4屏
Alt+Shift+8 垂直分屏-2屏
Alt+Shift+9 垂直分屏-3屏
Ctrl+K+B 开启/关闭侧边栏
F11 全屏模式
Shift+F11 免打扰模式
```
