## [2025-05-04] 解决树莓派无法远程连接

**问题描述**
树莓派Ubuntu 22.04安装Todesk进行远程连接时，电脑进度卡在100%

**原因分析：**
截止目前时间，Todesk只支持X11协议，没有适配最新的Wayland协议，需要把窗口系统调整为X11

**解决方案：**
```bash
# step 1
sudo nano /etc/gdm3/custom.conf
# step 2 
删除 #WaylandEnable=false 前的 # 

# step 3 
reboot
```

**参考资料：**
- [Ubuntu系统Todesk进度卡在100%，RealVNCserver无UI](https://www.bilibili.com/opus/909144121606668296)
给出了问题的解决方案，以及Wayland和X11之间的差别

 
