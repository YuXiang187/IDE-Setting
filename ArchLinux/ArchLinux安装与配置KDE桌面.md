# ArchLinux安装与配置KDE桌面

### 1.更新系统

```bash
pacman -Syyu
```

### 2.创建新用户

```bash
useradd -m -g users -G wheel -s /bin/bash yuxiang # 创建用户yuxiang
```

设置密码：

```bash
passwd yuxiang # 给用户yuxiang设置密码
```

### 3.编辑新用户权限

```bash
EDITOR=vim visudo
```

按下<kbd>/</kbd>搜索`wheel`，然后<kbd>Enter</kbd>

定位到`# %wheel ALL=(ALL) ALL`，在`#`号下按**2**次<kbd>X</kbd>删除注释

按<kbd>Esc</kbd>输入`:wq`退出Vim

### 4.安装KDE桌面环境

```bash
pacman -S plasma-meta konsole dolphin bash-completion
```

按**2**次<kbd>Enter</kbd>安装

将SDDM设置为开机自启

```bash
systemctl enable sddm
```

开启32位支持库以及ArchLinuxCN支持库

```bash
sudo vim /etc/pacman.conf
```

按下<kbd>Shift</kbd>+<kbd>G</kbd>到达文档末

光标往上，定位到一下行，然后删除下面文字前面的`#`号注释

```bash
[multilib]
Include = /etc/pacman.d/mirrorlist
```

再把下面文字的注释去掉

```bash
[custom]
SigLevel = Optional TrustAll
Server = file:///home/custompkgs
```

再把`[custom]`改成`[archlinuxcn]`，删除`SigLevel`一行，并更换为中科大的源

```bash
[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

按下<kbd>Esc</kbd>输入`:wq`保存退出

退出后再更新一下新添加的内容

```bash
sudo pacman -S archlinuxcn-keyring
```

```bash
pacman -Syyu
```

好了，输入`reboot`重启！

### 5.进入桌面的配置

进入桌面后，打开`Konsole`终端进行操作

安装必要插件：

* 安装ntfs-3g：

  ```bash
  sudo pacman -S ntfs-3g
  ```

* 安装开源字体

  ```bash
  sudo pacman -S adobe-source-han-serif-cn-fonts wqy-zenhei
  ```

  ```bash
  sudo pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji noto-fonts-extra
  ```

* 安装Chrome

  ```bash
  sudo pacman -S google-chrome
  ```


将系统设置为中文

在启动菜单上找到`System Settings`并启动，点击`Regional Settings`，在`Language`一栏点击`Add languages...`按钮，拉到下面找到`简体中文`，点击后点`Add`，把它拉到最上面，点击`Apply`按钮，完成，`Log out`重新登录即可看到效果

### 6.配置中文输入法

安装fcitx输入法

```bash
sudo pacman -S fcitx5-im
```

安装fcitx提供的中文输入包

```bash
sudo pacman -S fcitx5-chinese-addons
```

安装主题皮肤

```bash
sudo pacman -S fcitx5-material-color
```

配置：进入`系统设置`，单击`区域设置`，单击`输入法`，单击`运行 Fcitx`，点击`添加输入法...`，在列表框中找到`Pinyin`并点击`添加`，再点一下`pinyin`旁边的`配置`按钮，勾选`在程序中显示预编辑文本`、勾选`启用云拼音`，返回输入法配置页面

单击`配置附加组件...`，在“界面”列表中找到`Classic User Interface`，点击它旁边的设置图标按钮，在主题下拉列表中选择`Material-Color-Blue`（根据个人喜好）

设置环境变量，让其它程序也能使用这个中文输入法

```bash
vim ~/.pam_environment
```

输入：

```bash
INPUT_METHOD DEFAULT=fcitx5
GTK_IM_MODULE DEFAULT=fcitx5
QT_IM_MODULE DEFAULT=fcitx5
XMODIFIERS DEFAULT=\@im=fcitx5
```

按下<kbd>Esc</kbd>输入`:wq`保存退出

接下来设置输入法开机启动：

点开启动菜单的`系统设置`，点击`开机和关机`，找到`自动启动`，点击`添加程序`，输入`fcitx`，单击`fcitx 5`后单击确定退出，完成，`Log out`重新登录即可看到效果

好了，按下<kbd>Ctrl</kbd>+<kbd>空格</kbd>来切换输入法

### 4.安装显卡驱动

#### Intel核心显卡

```bash
sudo pacman -S xf86-video-intel mesa lib32-mesa vulkan-intel lib32-vulkan-intel
```

#### NVIDIA核心显卡

```bash
sudo pacman -S nvidia nvidia-settings nvidia-utils lib32-nvidia-utils opencl-nvidia lib32-opencl-nvidia
```

如果是GeForce630以下到GeForce400系列的老卡，上述包除了不要安装`nvidia`，其余照样安装，并且安装`nvidia-390xx-dkms`（AUR）及其32位支持包

```bash
yay -S nvidia-390xx-dkms nvidia-390xx-utils lib32-nvidia-390xx-utils
```

再老的显卡直接使用开源驱动即可：

```bash
sudo pacman -S mesa lib32-mesa xf86-video-nouveau
```

若为Intel核显+Nvidia独显的笔记本，除上述的包，安装`optimus-manager`，可以在核心显卡和独立显卡间轻松切换

```bash
yay -S optimus-manager optimus-manager-qt
```

在你切换显卡模式**前**需要进行的配置：

* I卡N卡的modeset选项都去掉勾选
* 切换到英特尔核显模式前，需要选择intel，不要选modesettings模式，否则会黑屏+混成不能开启
* hybird模式中添加的三个环境变量，在切换到其他模式之前一定要去掉，否则会黑屏，切换不到intel

### 5.KDE桌面美化

> 原则：美化不应该付出大量的时间折腾，既没有实际用处，也没有意义。花最少的时间完成性价比最高的美化才是最好的

安装yay源

```bash
sudo pacman -S yay
```

下载代理器

```bash
sudo pacman -S proxychains-ng
```

编辑代理文件

```bash
sudo vim /etc/proxychains.conf
```

按<kbd>Shift</kbd>+<kbd>G</kbd>把光标拖到文档末，找到`ProxyList`，在文档末添加：

（将socks4 127.0.0.1 9050改为下面的端口）

```bash
socks5 127.0.0.1 1080
```

按下<kbd>Esc</kbd>输入`:wq`保存退出

通过代理打开代理

```bash
proxychains systemsettings5 #通过代理打开系统设置
```

`系统设置>全局主题>获取新的全局主题`搜索`layan`，进行设置即可

设置壁纸可以在桌面右键鼠标菜单进行壁纸设置

设置系统图标：`系统设置>图标>图标>获取新图标主题`，搜索`Tela-icon-theme`即可进行设置

设置SDDM（登录界面）主题：`系统设置>开机和关机>登录屏幕（SDDM）>获取新登录屏幕`设置`layan`即可

任务栏插件：网速显示组件`Netspeed widget`强烈建议安装，很实用

混成器（毛玻璃效果）：`系统设置>显示和监控>混成器`，选择平滑，选择OpenGL2.0

终端样式设置：打开konsole，`设置>编辑当前方案>外观`，选择`Red-Black`应用确认即可

安装Latte Dock插件

```bash
sudo pacman -S latte-dock
```

安装Simple System Monitor插件：右键点击桌面空白部分→`添加部件`→搜索 Simple System Monitor→`安装`

动态壁纸可以选择`Smart Video Wallpaper`插件

这里列一下Arch Linux常用软件列表：

| 软件名               | 描述                     |
| -------------------- | ------------------------ |
| FlameShot            | 火焰截图                 |
| Motrix               | 下载工具，支持百度网盘   |
| Audious              | 音乐播放器               |
| VLC                  | 媒体播放器               |
| Chrome               | 浏览器                   |
| WPS                  | 代替Office               |
| SimpleScreenRecorder | 录屏                     |
| Sublime Text         | 文本编辑器               |
| Typora               | Markdown编辑工具         |
| KDE Connect          | Linux+Android强大交互    |
| XMind                | 思维导图                 |
| QQ for Linux         | 腾讯万年不更新的Linux QQ |
| GIMP                 | 类似Ps                   |
| shotcut              | 类似Pr                   |
| BleachBit            | 清理工具                 |
| XDM                  | 类似IDM下载工具          |

一些通过Wine运行的Windows软件安装方法：

QQ：

```bash
sudo pacman -S com.qq.im.deepin
```

TIM：

```bash
sudo pacman -S deepin.com.qq.office
```

QQ轻聊版：

```bash
sudo pacman -S deepin.com.qq.im.light
```

微信：

```bash
sudo pacman -S com.qq.weixin.deepin
```

请给他们点个赞：

**Deepin：**deepin-wine环境及其应用容器包含了deepin公司的大量心血。安装只需一瞬间，开发却可能是寒来暑往、披星戴月、夜以继日的奋斗才能搞定。请自觉用各种方式支持deepin，不止是物质上的，更是精神上的

**wszqkzqk同学：**这里是他的[Github](https://github.com/wszqkzqk)库和[码云](https://gitee.com/wszqkzqk/)库。他还未成年，对deepin产品一腔热忱。一放暑假，就搞定了Ubuntu的一键deepin wine qq包。没有他的移植工作，我在ArchLinux系统里可能就用不上稳定好用的TIM/QQ

接下来的一些好玩的配置就等着你自己探索了:-)
