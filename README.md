# windows10 使用 wsl2 搭建开发环境。


1. windows设置 > 应用 > 程序和功能 > 启用或关闭windows功能
勾选
虚拟机平台、
勾选
windows subsystem for linux
点击确定。
或者使用以下命令开启虚拟机平台和wsl

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
### 注意，使用命令安装虚拟机平台，一定要多等一会，再重新启动计算机，不然会安装不完整，之后安装linux子系统会报错。
安装完成后，重新启动。

2. 安装wsl2 linux [内核更新包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)。
```
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
```
3. 打开命令行，开启wsl2
```
wsl --set-default-version 2
```
4. 安装linux。可以使用 microsoft store ，Windows的软件商店查找安装。或使用命令安装。
#### 查看可用的linux子系统
```
wsl --list --online
```
#### 我使用的是ubuntu-18.04。
```
wsl --install -d ubuntu-18.04
```
#### 使用wsl命令进入linux，或者在开始菜单中点击ubuntu-18.04进入。第一次进入，会完成安装和设置密码。
5. [安装vscode](https://code.visualstudio.com/)
```
https://code.visualstudio.com/
```
6. 安装vscode插件
```
Remote WSL
Remote SSH
Remote Containers
```
[其他推荐插件。](https://github.com/slkde/vscode)
```
https://github.com/slkde/vscode
https://gitee.com/slkde/vscode
```

7. [安装docker](https://www.docker.com/)
```
https://www.docker.com/
```
8. 在docker中开启wsl2
```
docker > settings > 勾选Use the WSL 2 based engine 点击apply

docker > settings > Resources > WSL INTEGRATION 开启ubuntu-18.04
```

### 说明：
1. 在windows中安装vscode之后，在linux中也可以使用，code .
2. windows开机后，必须先开启linux子系统。然后启动docker。不然docker会报错。
3. linux中挂载windows系统的位置
```
/mnt/c
/mnt/d
/mnt/e
```
4. 推荐在linux文件系统中使用开发环境。跨系统读写文件系统会很慢。
5. 因为windows不区分大小写。所以不要在linux子系统中使用windows分区来编译软件。

## wsl中挂载linux分区：
说明：正式版windows系统wsl2不支持挂载分区。请先升级到windows预览版。目前不能单独挂载分区。所以需要安装第二块硬盘。
1. 查看硬盘。
```
wmic diskdrive list brief

Caption          DeviceID       Model   Partitions  Size
...         \\.\PHYSICALDRIVE2   ...        4       ...
...         \\.\PHYSICALDRIVE1   ...        3       ...
...         \\.\PHYSICALDRIVE0   ...        5       ...
```
2. 挂载硬盘或分区都可以。在/mnt/PHYSICALDRIVE下。
```
挂载硬盘
wsl --mount \\.\PHYSICALDRIVE2
挂载分区
wsl --mount \\.\PHYSICALDRIVE2 --partition 4
```
3. 没有第二块硬盘？可以使用虚拟磁盘vmdk：
## 其他说明：
终端推荐使用Windows Terminal，在windows测试版里默认安装。
```
https://github.com/microsoft/terminal/releases
```
以前用过linux桌面版在实体机上做为开发环境。实在不习惯。
mac os 中开发也还好。对docker支持不错。
但还是windows用起来更习惯一些。
在windows中打开vscode时，最好点击左下角，点击连接wsl。
remote-wsl reopen folder in wsl

## 你的C盘太小？
把linux子系统放入E盘
1. 导出系统
```
wsl --export ubuntu-18.04 e:\wsl-ubuntu-18.04.tar
```
2. 删除当前系统
```
wsl --unregister ubuntu-18.04
```
3. 导入系统
```
wsl --import ubuntu-18.04 e:\wsl\ubuntu-18.04 e:\wsl-ubuntu-18.04.tar --version 2
```
4. 重新设定默认登陆用户名
```
cd C:\Users\你的windows用户名\AppData\Local\Microsoft\WindowsApps\

ubuntu1804.exe config --default-user 你之前用的linux子系统用户名
```
============================================================
```
wsl --list --verbose
```