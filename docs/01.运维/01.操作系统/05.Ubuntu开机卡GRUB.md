---
title: Ubuntu开机卡GRUB
date: '2024-02-22 11:13:03'
meta:
  - name: keywords
    content: Area/Linux
tags:
  - Area/Linux
categories:
  - 操作系统
permalink: /post/ubuntu-boot-card-grub-z2dpofl.html
author:
  name: Zhou
  link: https://github.com/Abner1zhou
---


<!-- more -->


# Ubuntu开机卡GRUB

<span style="font-weight: bold;" data-type="strong">操作系统：Ubuntu 18.04</span>

Ubuntu 18.04开机显示`grub>`​提示符时，意味着GRUB引导程序无法找到配置文件，无法正常引导进入操作系统。这通常发生在更新、配置错误或磁盘错误后。

> 本次故障原因：意外断电关机

‍

1. <span style="font-weight: bold;" data-type="strong">尝试手动引导系统</span>：

    * 在`grub>`​提示符下，你可以尝试手动指定引导参数。首先，使用`ls`​命令查看可用的分区和设备：

      ```
      grub> ls
      ```
    * 查找包含Ubuntu安装的分区。这通常是形式为`(hdX,Y)`​的设备，<span style="font-weight: bold;" data-type="strong">其中</span>​`<span style="font-weight: bold;" data-type="strong">X</span>`​<span style="font-weight: bold;" data-type="strong">和</span>​`<span style="font-weight: bold;" data-type="strong">Y</span>`​<span style="font-weight: bold;" data-type="strong">分别代表硬盘编号和分区编号</span>。
    * 然后，设置根分区、加载内核和initrd镜像：

      ```
      grub> set root=(hdX,Y)
      grub> linux /boot/vmlinuz-<version>-generic root=/dev/sdXY ro
      grub> initrd /boot/initrd.img-<version>-generic
      grub> boot
      ```

      替换`<version>`​为你的内核版本，`sdXY`​为实际的设备名（例如，`sda1`​）。
    * <span style="font-weight: bold;" data-type="strong">注：如果使用了LVM分区</span>，则将第二行命令改为`grub> linux /boot/vmlinuz-linux root=/dev/mapper/vgname-lvname ro`​  
      ​`vgname-lvname`​ <span style="font-weight: bold;" data-type="strong">修改为实际分区名</span>
2. <span style="font-weight: bold;" data-type="strong">修复GRUB</span>：

    * 如果手动引导成功，你应该考虑修复GRUB。这通常涉及到从Ubuntu安装介质（USB或光盘）启动进入“试用”模式，打开一个终端，然后安装并更新GRUB。
    * 使用以下命令安装和更新GRUB：

      ```
      sudo mount /dev/sdXY /mnt  # 挂载根分区
      sudo grub-install --root-directory=/mnt /dev/sdX  # 安装GRUB
      sudo grub-install --boot-directory=/mnt/boot /dev/sdX  # 若上一个命令不工作尝试这个
      sudo update-grub  # 更新GRUB配置
      ```

      请将`sdX`​和`sdXY`​替换为实际的设备名。
3. <span style="font-weight: bold;" data-type="strong">检查磁盘错误</span>：

    * 如果上述方法不起作用，可能是因为磁盘错误。你可以使用`fsck`​工具检查和修复文件系统错误。注意，在运行`fsck`​之前，确保分区没有挂载。
4. <span style="font-weight: bold;" data-type="strong">重新安装Ubuntu</span>：

    * 如果所有其他尝试都失败了，最后的手段可能是备份重要数据（通过试用模式访问文件系统）并重新安装Ubuntu。

‍
