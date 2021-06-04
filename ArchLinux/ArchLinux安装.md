# ArchLinux安装

> BIOS安装Arch Linux真的是折腾死我了QAQ
>
> 下面的文章将基于Arch Linux的Wiki详细介绍安装过程
>
> 我强烈不建议新手折腾Arch Linux，如果你是新手的话，我还是推荐你去安装基于Arch Linux的[Manjaro](https://manjaro.org/)系统

### 1.验证启动模式

验证是否为UEFI模式启动，否则就是BIOS模式启动

```bash
ls /sys/firmware/efi/efivars
```

### 2.检测网络连接

```bash
ping www.baidu.com -c3 # Ping百度3次后自动退出
```

也可以按下<kbd>Ctrl</kbd>+<kbd>C</kbd>退出Ping

***

#### 附：无线网络连接

```bash
iwctl # 进入iwctl
```

进入后：

```bash
device list # 看看你的网卡叫什么名字
```

```bash
station wlan0 scan # wlan0是无线网卡名
```

```bash
station wlan0 get-networks # 查看已被扫描的无线网络
```

```bash
station wlan0 connect CMCC # wlan0是无线网卡名，CMCC是网络名
```

接下来输入密码后就连接成功了，输入`exit`退出

如果还不能联网输入下面的命令试试：

```bash
systemctl start dhcpcd # 输入这个再输入ping试试
```

### 3.更新系统时间

同步系统时间：

```bash
timedatectl set-ntp true
```

可以使用 `timedatectl status` 检查服务状态

### 4.更换镜像源

使用Vim编辑：

```bash
vim /etc/pacman.d/mirrorlist
```

1. 按下<kbd>/</kbd>，输入`China`，按下<kbd>Enter</kbd>，定位到CN源，定位到你最喜欢的源那一行

2. 按**2**次<kbd>D</kbd>键，剪切镜像源

3. 按**2**次<kbd>G</kbd>，返回文件最上面

4. 在最上面选择一个位置按<kbd>P</kbd>即可粘贴

5. 按<kbd>Esc</kbd>，再输入`:wq`退出Vim

### 5.设置磁盘类型

可以先使用`lsblk`来查看当前磁盘状况

使用`parted`命令来对磁盘进行操作：

```bash
parted /dev/sda # /dev/sda是要操作的磁盘
```

进去后，输入：

```bash
mktable
```

它问你要什么类型的磁盘？输入`gpt`

操作完成后输入`quit`退出

### 6.磁盘分区

进入分区操作界面：

```bash
cfdisk /dev/sda
```

按下<kbd>Enter</kbd>，进入分区操作界面

* 这是UEFI启动的分区的一个例子：

  | Device    | Size | Size Type        |
  | --------- | ---- | ---------------- |
  | /dev/sda1 | 300M | EFI System       |
  | /dev/sda2 | 2G   | Linux swap       |
  | /dev/sda3 | 25G  | Linux filesystem |
  | /dev/sda4 | 60G  | Linux filesystem |

* 这是BIOS启动的分区的一个例子：

  不建议给`/boot`目录分区，因为ArchLinux是采用滚动更新政策，所以`/boot`目录会越用越大，万一挤满了，内核就无法安装，引发问题，上面的`EFI System`分区和下面的`BIOS boot`分区不是`/boot`分区！不要搞混了！
  
  | Device    | Size | Size Type        |
  | --------- | ---- | ---------------- |
  | /dev/sda1 | 1M   | BIOS boot        |
  | /dev/sda2 | 2G   | Linux swap       |
  | /dev/sda3 | 25G  | Linux filesystem |
  | /dev/sda4 | 60G  | Linux filesystem |

设置完成后，将光标移动到`Write`下，按下<kbd>Enter</kbd>，然后输入`yes`

将光标移动到`quit`下退出

可以输入`fdisk -l`查看磁盘分区情况

### 7.格式化磁盘

#### 给UEFI

格式化根目录分区：

```bash
mkfs.ext4 /dev/sda3 # 将sda3（也就是根目录）格式化成ext4类型
```

格式化家目录分区：

```bash
mkfs.ext4 /dev/sda4 # 将sda4（也就是家目录）格式化成ext4类型
```

格式化EFI分区：

```bash
mkfs.vfat /dev/sda1 # 将sda1（也就是EFI分区）格式化成vfat类型
```

格式化swap分区：

```bash
mkswap -f /dev/sda2 # 将sda2（也就是swap分区）格式化成swap类型
```

```bash
swapon /dev/sda2 # 启动swap
```

#### 给BIOS

将根目录格式化为ext4：

```bash
mkfs.ext4 /dev/sda3 # 将sda3（也就是根目录）格式化成ext4类型
```

格式化家目录分区：

```bash
mkfs.ext4 /dev/sda4 # 将sda4（也就是家目录）格式化成ext4类型
```

格式化swap分区：

```bash
mkswap -f /dev/sda2 # 将sda2（也就是swap分区）格式化成swap类型
```

```bash
swapon /dev/sda2 # 启动swap
```

### 8.挂载磁盘

#### 给UEFI

挂载根目录：

```bash
mount /dev/sda3 /mnt
```

挂载家目录：

```bash
mkdir /mnt/home # 创建家目录
mount /dev/sda4 /mnt/home # 挂载home目录
```

挂载EFI分区：

```bash
mkdir /mnt/boot # 创建boot目录
mkdir /mnt/boot/EFI # 创建EFI目录
mount /dev/sda1 /mnt/boot/EFI # 挂载EFI目录
```

#### 给BIOS

挂载根目录：

```bash
mount /dev/sda3 /mnt
```

挂载家目录：

```bash
mkdir /mnt/home # 创建home目录
mount /dev/sda4 /mnt/home # 挂载home目录
```

### 9.安装ArchLinux

安装必备的软件包：

```bash
pacstrap /mnt base linux linux-firmware
```

安装功能性软件：

```bash
pacstrap /mnt dhcpcd iwd vim sudo
```

### 10.配置ArchLinux

生成fstab文件：

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

**强烈建议**使用`cat /mnt/etc/fstab`检查一下文件是否正确

进入新系统：

```bash
arch-chroot /mnt
```

设置时区：

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

同步硬件时钟：

```bash
hwclock --systohc
```

编辑`/etc/locale.gen`设置本地地址：

```bash
vim /etc/locale.gen
```

设置步骤：

1. 按下<kbd>/</kbd>输入`en_US`回车，再按<kbd>N</kbd>键查找下一个目标，直到找到

   `#en_US.UTF-8 UTF-8`，光标移动到`#`下面，按下<kbd>x</kbd>键把注释去掉

2. 操作完后按下<kbd>Esc</kbd>后输入`:wq`保存并退出Vim

接着执行以下命令生成Locale信息：

```bash
locale-gen
```

接着往`locale.conf`输入一些内容：

```bash
echo 'LANG=en_US.UTF-8' > /etc/locale.conf
```

**强烈建议**使用`cat /etc/locale.conf`检查一下文件是否正确

设置主机名：

```bash
echo yuxiang-PC > /etc/hostname
```

接着向`/etc/hosts`文件添加以下内容：

```bash
vim /etc/hosts
```

```bash
127.0.0.1	localhost
::1		localhost
127.0.1.1	yuxiang-PC.localdomain	yuxiang-PC # 主机名.本地域名 主机名
```

空出来的部分是<kbd>Tab</kbd>！！

设置Root用户密码：

```bash
passwd root
```

安装微码：

```bash
pacman -S intel-ucode # Intel的CPU
```

```bash
pacman -S amd-ucode # AMD的CPU
```

### 11.安装引导程序

> **警告：** 这是安装的最后但也至关重要的一步，请按上述指引正确安装好引导加载程序后再重新启动。否则将无法正常进入系统

#### 给UEFI

安装必备的包：

```bash
pacman -S grub efibootmgr
```

给UEFI系统安装Grub：

```bash
grub-install --target=x86_64-efi --efi-directory=/boot/EFI --bootloader-id=GRUB
```

生成配置文件：

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

#### 给BIOS

安装必备包：

```bash
pacman -S grub
```

给BIOS系统安装Grub：

```bash
grub-install --target=i386-pc --recheck /dev/sda # /dev/sda是要装的磁盘
```

其中 `/dev/sda` 是要安装 GRUB 的磁盘，而 **不是** 分区 `/dev/sda1`

生成配置文件：

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

***

完毕，输入`exit`退回安装环境

使用`umount -R /mnt`卸载分区

输入`reboot`重启！**重启后要拔掉U盘！**

### 12.给新系统设置网络

登录Root账户进入系统后，输入：

```bash
systemctl enable dhcpcd # 设置开机自启
```

现在立刻启动dhcpcd：

```bash
systemctl shart dhcpcd # 立即启动！
```

稍等几秒后，使用`ping`检测网络：

```bash
ping www.baidu.com -c3 # Ping百度3次后自动退出
```

也可以按下<kbd>Ctrl</kbd>+<kbd>C</kbd>退出Ping

#### 附：命令行查看系统信息

可以安装`neofetch`这个软件包来通过命令行查看系统信息：

```bash
pacman -S neofetch
```

```bash
neofetch # 查看系统信息
```
