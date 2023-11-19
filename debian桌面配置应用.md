#  Linux 安装软件

##  QQ MUSIC

需要加上 --no-sandbox完整的图标参数如下

``` shell
sudo vim /usr/share/applications/qqmusic.desktop
 [Desktop Entry]
 Name=qqmusic
 Exec=/opt/qqmusic/qqmusic --no-sandbox %U
 Terminal=false
 Type=Application
 Icon=qqmusic
 StartupWMClass=qqmusic
 Comment=Tencent QQMusic
 Categories=AudioVideo;

```
## 挂载硬盘
```shell
##查看有那些硬盘
$ df -h
# 创建挂在点
 /home/kane/D_driver
## 挂载硬盘
sudo mount /dev/sdb1  /home/kane/D_drive_letter
#查看磁盘的UUID
blkid /dev/sdb1
#开机自动挂载 保存到文件最后一行
sudo vim /etc/fstab
UUID=82513648-8c70-458c-a6b8-2739fef1cb5b /home/kane/D_driver       ext4    defaults        0 0 
```

## steam
```shell 
# 安装steam
sudo apt install steam

#卸载Steam
sudo apt-get remove steam steam-launcher
sudo apt-get purge steam steam-launcher
rm -rf ~/.local/share/Steam && rm -rf ~/.steam
```
## wps 缺少字体
```shell 
下载字体
链接: https://pan.baidu.com/s/1UkjW-JqEvnQFOOgVg4Cb3Q 提取码: glfs

将字体复制到文件下：/usr/share/fonts/wps-office
sudo cp /home/kane/D_drive_letter/Downloads/wps_symbol_fonts /usr/share/fonts/wps-office

```

 ## dpkg 依赖安装错误
```shell 
sudo dpkg -C
sudo apt-get install check
udo apt --fix-broken install
sudo dpkg -i --force-overwrite /var/cache/apt/archives/*.deb  
#包自动配置触发器和一些文件   测试信息
sudo dpkg --configure -a
#自动下载修复依赖包并且安装
sudo apt --fix-broken install
#可以看到有错误，错误是需要覆盖某个doc文件内容，doc又不重要，覆盖就覆盖了呗。。。
sudo dpkg --force-overwrite -i libwrap0_7.6.q-27_i386.deb

```