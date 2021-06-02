# ArchLinux日积月累

### 1.解压命令

* tar.gz

  ```bash
  tar -xzvf file
  ```

* tar.xz

  ```bash
  tar -xvJf file
  ```

### 2.强制杀死进程

1. 用`ps`命令查看进程

   ```bash
   ps -ef
   ```

   ```bash
   ps -ef | grep firefox # 指定程序名
   ```

2. 用`kill`命令杀死进程

   ```bash
   kill -s 9 1827
   ```

   其中`9`是给程序发送信号：强制、尽快终止进程

### 3.设置任务栏透明度

安装`InkScape`

```bash
sudo pacman -S inkscape
```

打开主题文件夹编辑svg即可

### 4.Wine QQ/Wine Wechat出现中文乱码

把Windows下的Font文件夹下的宋体和微软雅黑复制到/usr/share/fonts/windows目录中（没有新建）

编辑配置文件：

```bash
sudo vim /opt/deepinwine/tools/run.sh 
```

然后，找到 WINE_CMD=“deepin-wine” 中间添加 “LC_ALL=zh_CN.UTF-8”

```bash
WINE_CMD="LC_ALL=zh_CN.UTF-8 deepin-wine"
```

`run_v2.sh`也作同样的修改，重启系统
