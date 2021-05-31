# B站缓存合并为MP4

> 下载ffmpeg工具

FFmpeg 是一款免费开源的转换音频视频格式的程序工具，下载地址：

http://www.121down.com/soft/softview-103719.html#downaddress

复制后在浏览器打开，按照提示下载即可，下载解压后，依次点击，进入如下界面

![img](https://pics7.baidu.com/feed/472309f790529822f784a12ffeb498cd0b46d428.jpeg?token=9e5f5eef001d39ef1e091d6d57ea79d6)

点击bin，进入如下界面

![img](https://pics3.baidu.com/feed/d31b0ef41bd5ad6ec301ff45a6b5daddb7fd3c2d.jpeg?token=59b7a58e2dcf4a81626600e28e4005e3)

把刚才桌面上的audio. m4s文件和video文件拖到此界面，如图

![img](https://pics4.baidu.com/feed/1c950a7b02087bf4f9b54670dbadb42a10dfcf57.jpeg?token=f19dad65394876f0fc7f131f02455f18)

然后，在空白处按住shift键，然后单击鼠标右键，选择在此处打开powershell窗口

![img](https://pics6.baidu.com/feed/9f2f070828381f30f266419b8a7faf0e6e06f00e.jpeg?token=f410b1a38f5fea855fd27c8377c402e7)

出现如下界面

![img](https://pics5.baidu.com/feed/fcfaaf51f3deb48f31a95266dc61d92f2ff578cf.jpeg?token=30d4998019a6f28961be29d2489ff30e)

复制“.\ffmpeg.exe -i video.m4s -i audio.m4s -codec copy Output.mp4”

这串代码，然后在这个蓝色界面内单击右键，将这款代码粘贴进去，如图

![img](https://pics5.baidu.com/feed/1e30e924b899a901520d999538ebe97d0308f5da.jpeg?token=6c03f0b6a4b1a20494a9ddafb73f1c41)

然后按回车键，出现如下代码即表示转换成功

![img](https://pics0.baidu.com/feed/a9d3fd1f4134970a03d55e0db1b432cea6865df6.jpeg?token=a23b6e978341ad716f5b9b3f1e76775c)

此时我们切换到刚才的bin文件夹，发现生成了一个output文件，此文件就是刚才的audio. m4s，video. m4s合成的mp4文件

![img](https://pics7.baidu.com/feed/a5c27d1ed21b0ef4ac28bb84f9bab2dc83cb3ea8.jpeg?token=b5d887de48e5311924746264d307a581)

我们可以发现output文件的大小近似等于两个m4s文件的大小之和。说明用这种方法可以无损合成，这个output文件与你原先缓存的视频完全相同。
