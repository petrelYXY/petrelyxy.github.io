---
layout: post
title: "gpu并行编程:cuda环境和NVIDIA驱动"
date:  2017-04-11 09:53:20 +0800
desc: "Always be fimiliar with these basic skills"
keywords: "anime"
categories: [MESS]
tags: [cuda， Nvidia，gpu]
icon: icon-html
---
### 一. 安装CUDA.
 1. 安装cuda
  1. 在nvidia官网下载cuda toolkit，例如下载runfile类型。安装步骤。根据参考资料完成选择：
	Run `sudo sh cuda_8.0.61_375.26_linux.run`
	Follow the command-line prompts
  2. 因为在linux系统中默认安装了nouveau驱动(nvidia开源驱动)，会与cuda使用的驱动冲突，所以需要禁用nouveau驱动。按照参考资料-4.1.2.2. Runfile Installer来进行。
Q1：按照教程安装CUDA环境后可以编译样例，但无法运行，报错为驱动版本对运行版本无效，更换驱动后黑屏，删除驱动重装无效。原因在于我的笔记本为双显卡，需要使用nvidia-prime，来执行默认驱动选取。(当dpkg中有nvidia-settings，也可以执行它来切换)。
  安装nvidia-prime和nvidia-settings后发现默认驱动是nouveau，手动更改无效，还是会自动选取nouveau，改blacklist.conf将其加入黑名单也无效，所以问题变成了如何禁用nouveau驱动而使用安装的nvidia驱动。

问题列表：
  1. 运行编译好的cuda样例出现问题：CUDA driver version is insufficient for CUDA runtime version
  2. 下载新的cuda8.0安装文件 检验checksum又不一致。damn
  3. 安装的nvidia-settings中无法识别管理我的N卡和集成显卡。

### 重新安装
  1. 改变默认驱动为X.Org Xserver(在系统设置->软件和更新->附加驱动；或搜索driver->附加驱动)，在本机上可以看到 应当设为nouveau.
  2. 转入命令行模式(ctrl+alt+f1, aka tty1模式),log in; sudo service lightdm restart。if restart to lightdm,open a terminal and use the next step,if not,use tty1, as before;
  
  3. 删除Nvidia相关文件。
	sudo apt-get remove --purge nvidia-*
  4. 检查是否有nvidia相关文件残余
	dpkg --get-selection | grep nvidia  确保没有任何nvidia残留
  5. 再次查看附加驱动，选择最高可用的驱动。
  6. Go back to tty1 and issue sudo service lightdm restart,Lightdm should restart and you should have the latest driver, for your system. 


————
安装文件：cuda-tookit和Nvidia驱动和官方资料放在移动硬盘里了。

参考教程：
1. runfile安装和禁用nouveau驱动: http://blog.csdn.net/masa_fish/article/details/51882183
