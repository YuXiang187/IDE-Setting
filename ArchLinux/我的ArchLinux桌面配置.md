# 我的ArchLinux桌面配置

## 1.系统主题及面板配置

> 原则：美化不应该付出大量的时间折腾，既没有实际用处，也没有意义。花最少的时间完成性价比最高的美化才是最好的

### 系统设置及主题配置

> 主题设置

* 安装全局主题：Layan
* 窗口装饰元素：Layan-solid
* 图标：Tela blue Dark
* 光标：Breeze 微风
* 欢迎屏幕：Magenta
* SDDM：Layan

> 离线主题设置

* `/home/username/.local/share/plasma/desktoptheme`

  这是存放Plasma主题

* `/home/username/.local/share/plasma/look-and-feel`

  存放全局主题

* `/home/username/.local/share/plasma/plasmoids/`

  存放插件

* `/usr/share/sddm/themes/`

  这里存放SDDM的主题

我的离线主题下载：

* SDDM：`Layansddm.tar.xz`
* 图标：`Tela-blue.tar.xz`

链接：https://pan.baidu.com/s/1veSwvDnWsblauRSJZlScdQ

提取码：hnfh

> 桌面特效

* 对话框显隐过度
* 窗口惯性晃动
* 窗口粉碎动画
* 最小化过渡动画(神灯)
* 窗口打开/关闭动效：滑翔
* 虚拟桌面3D切换动效

> 虚拟桌面

* 主桌面
* 副桌面

> 锁屏

* 自动锁定屏幕：如果空闲`10`分钟
* 外观配置：Your Name

外观壁纸下载：

链接：https://pan.baidu.com/s/1Q_bTUDalPIoO6UJSYFTLCw

提取码：8vur

> 快捷键

* KWin：切换下一个桌面<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Tab</kbd>

> 显示和监控

* 混成器：选择平滑，选择OpenGL2.0
* 打开夜晚颜色

> 终端配置

* 透明度设置为40%

### 面板配置

> 顶部菜单栏面板从左到右如下布局：

* 应用程序菜单：设置为Arch Linux图标
* Application title：无窗口时设置显示文本“Arch Linux”
* 全局菜单
* 面板间距
* CPU1总使用率：设置饼图
* CPU2总使用率：设置饼图
* Netspeed Widget
* 查找
* 便利贴：设置半透明白字便利贴
* 系统托盘
* 数字时钟：仅显示时钟
* 锁定/注销：仅显示关机键

> 可选：Simple System Monitor、Smart Video Wallpaper

## 2.安装软件及其配置

### Latte - 底部栏

> 安装：

```bash
sudo pacman -S latte-dock
```

> 设置：

大小为`56`像素

> 固定的应用：

* Dolphin
* Google Chrome
* XMind
* 火焰截图
* BleachBit
* WPS 2019
* Typora
* KDE Connect
* VLC
* GIMP

### 火焰截图 - 截图

```bash
sudo pacman -S flameshot
```

在首选项自行设置

### Okular - PDF阅读器

```bash
sudo pacman -S okular
```

### Typora - 笔记

```bash
sudo pacman -S typora
```

安装主题：

`typora-onedark-theme-v1.09.zip`、`panda.zip`：

链接：https://pan.baidu.com/s/1TcFmheLhGgjxYx5eFc3Zsg

提取码：58dq

把“发送匿名使用数据”的复选框勾掉

### WPS Office - 办公

```bash
yay -S wps-office-cn ttf-wps-fonts wps-office-mui-zh-cn
```

> 让WPS Office使用GTK + UI（来自ArchLinux Wiki的解决方案）

* 修改启动desktop文件：

修改 `/usr/share/applications/` 下以 wps-office 开头的 desktop 文件：

**提示：** 如果你使用的 flatpak 安装的应用，请查看 `/var/lib/flatpak/exports/share/applications` 目录

找到 Exec 行，在 %f 前添加启动参数：

```
-style=gtk+
```

为避免软件更新后，修改被覆盖，可以选择拷贝所有需要修改的 desktop 文件到 `~/.local/share/applications/` 后，再做修改。

**注意：** 在修改 desktop 后请运行 `update-desktop-database ~/.local/share/applications/` 命令刷新菜单缓存（该命令的参数是存放已修改过的 desktop 文件的目录）

* 修改启动脚本

修改 /usr/bin/ 目录下的 et、wpp、wps 启动脚本文件

删除该行（如果有的话）：

```
gOptExt=
```

然后添加：

```
gOptExt="-style=gtk+"
export GTK2_RC_FILES=/usr/share/themes/Adwaita/gtk-2.0/gtkrc
```

**注意：** 在 export 参数中可以导入其他支持GTK2的主题，对于应用界面将会呈现不一样的效果

**注意：** 对于 金山 PDF （WPS PDF） 应用，可能存在启动脚本缺失的情况，请参考下节解决方案

* 手动修复 金山 PDF 启动脚本

金山 PDF 提供的启动脚本缺失了对 GTK 的自定义配置 可以在其启动脚本 /usr/bin/wpspdf 开始位置添加：

```
gOptExt="-style=gtk+"
export GTK2_RC_FILES=/usr/share/themes/Adwaita/gtk-2.0/gtkrc
```

并在其后的 run 函数中添加 `${gOptExt}`，修改后的 run 函数如下：

```
function run()
{
	if [ -e "${gInstallPath}/office6/${gApp}" ] ; then
		{ ${gInstallPath}/office6/${gApp} ${gOptExt} "$@"; } >/dev/null 2>&1
	else
		echo "${gApp} does not exist!"
	fi
}
```

**Note:** 由于每次升级可能导致文件修改遗失，可以考虑将 et、wpp、wps 文件复制到其他目录（例如：`~/.local/bin/`），并将其添加到 [Environment variables](https://wiki.archlinux.org/title/Environment_variables)

### XMind - 思维导图

```bash
yay xmind-2020
```

对于10.3.1-0版本提供的破解补丁：

链接：https://pan.baidu.com/s/1aGosEeeOEVtawFoNKDF0eA

提取码：vqve

### nomacs - 图像查看

```bash
sudo pacman -S nomacs
```

### VLC - 影音播放

```bash
sudo pacman -S vlc
```

### SimpleScreenRecorder - 录屏

```bash
sudo pacman -S simplescreenrecorder
```

### BleachBit - 系统清理

```bash
sudo pacman -S bleachbit
```

在“首选项” - “语言”中设置要清除的语言，注意要进入`root`账户

### KDE Connect - 手机互联

```bash
sudo pacman -S kdeconnect
```

### Sublime Text - 文本编辑

```bash
yay -S sublime-text-4
```

## 可选安装

> PSCS6的破解补丁下载：
>
> 链接：https://pan.baidu.com/s/1p7jb0ZROWh-ZWkqgJkF_mw
>
> 提取码：uvif

* shotcut：`sudo pacman -S shotcut`
* GIMP：`sudo pacman -S gimp`
* PSCS6：`yay -S com.pscs6.deepin`
* QQ：`yay -S com.qq.im.deepin`
* QQ轻聊版：`yay -S deepin.com.qq.im.light`
* TIM：`yay -S deepin.com.qq.office`
* 微信：`yay -S com.qq.weixin.deepin`
