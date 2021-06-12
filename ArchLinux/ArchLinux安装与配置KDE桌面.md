# ArchLinux安装与配置KDE桌面

> 下面是我在B站上学习如何安装与配置KDE桌面的笔记，B站上的链接如下：

* [【收藏向②】ArchLinux安装KDE桌面&Fcitx5输入法配置](https://www.bilibili.com/video/BV1Vk4y117jc)
* [【收藏向③】超简单Linux双显卡驱动教程/学不会来捶我/Optimus Manager](https://www.bilibili.com/video/BV1vK4y187Ww)
* [【收藏向④】KDE桌面美化教程](https://www.bilibili.com/video/BV1Ua4y157Qa)

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

再把下面文字的注释去掉，如：

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

* 安装基本环境（`yay`命令需要用到）

  ```bash
  sudo pacman -S git base-devel
  ```

* 安装Chrome

  ```bash
  sudo pacman -S archlinuxcn-keyring
  ```

  ```bash
  sudo pacman -S google-chrome
  ```


将系统设置为中文

在启动菜单上找到`System Settings`并启动，点击`Regional Settings`，在`Language`一栏点击`Add languages...`按钮，拉到下面找到`简体中文`，点击后点`Add`，把它拉到最上面，点击`Apply`按钮，完成

再将Windows下的字体文件`msyh.ttf`、`msyhbd.ttf`、`simhei.ttf`复制到`/usr/share/fonts/`目录下

`Log out`重新登录即可看到效果

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

警告：装上`xf86-video-intel`在某些机器上运行录屏软件会闪退，如果出现闪退，那就不要安装

#### NVIDIA独立显卡

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

### 5.安装yay源

直接在`Konsole`下输入下面这条命令即可安装：

```bash
sudo pacman -S yay-git
```

### 6.安装flatpak

直接在`Konsole`下输入下面这条命令即可安装：

```bash
sudo pacman -S 
```

可以去[Flathub](https://www.flathub.org/home)官网下载软件库引用，然后在终端用下面的命令安装：

```bash
sudo flatpak install packagename.flatpakref
```

卸载软件，如卸载WPS：

```bash
sudo flatpak uninstall com.wps.Office
```
