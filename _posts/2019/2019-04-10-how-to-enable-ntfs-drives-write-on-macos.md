---
layout: post
title:  "如何在 macOS 上开启 NTFS 硬盘的写权限"
date:   2019-04-10 20:54:36 +0800
author: Coshin
comments: true
category: 程序人生
tags: macOS fstab NTFS
---
macOS 对 NTFS 格式的硬盘默认只支持读操作，据说是版权原因。
事实上可以通过编辑系统配置文件 fstab 开启 NTFS 硬盘的写权限。

![ntfs-write-mac-osx](https://cdn.osxdaily.com/wp-content/uploads/2013/10/ntfs-write-mac-osx.jpeg){:.border}

文件系统表 fstab（file system table）用于描述文件系统的静态信息，是 UNIX 和类 UNIX 计算机系统上常见的系统配置文件。
该文件通常列出所有可用磁盘分区，并指明如何初始化它们。
mount 命令在引导时自动读取 fstab 文件以确定整个文件系统结构，直至当用户执行 mount 命令修改该结构。
传统的 UNIX 总是允许特权用户（root 用户和 wheel 组的用户）在没有 fstab 条目的情况下挂载或卸载设备。

## 1. fstab 文件中各字段说明

> LABEL=device-spec mount-point fs-type options dump pass
> * device-spec: 设备名，UUID。
> * mount-point: 安装后访问设备内容的位置；对于交换分区或文件，设置为 none。
> * fs-type: 要挂载文件系统的类型。
> * options: 描述文件系统各种其他方面，例如是否在引导时自动挂载，用户可以挂载或访问，是否可以写入或只读，大小等等；特殊选项默认值是指一组预定的基于文件系统类型的选项。
> * dump: 一个数字，指明转储程序是否应该以及多久备份文件系统；0 表示永远不会自动备份文件系统。
> * pass: 一个数字，指明 fsck 程序在引导时检查设备是否有错误的顺序；1 表示对于 root 文件系统，2（在 root 后检查）或 0（不检查）表示对于所有其他设备。

**最后两个字段缺省值为 0。第一、二、四字段中的空格字符由八进制的字符代码 `\040` 表示，参照 ASCII 码表。**

由于 `/etc/fstab` 中的文件系统最终使用 mount 挂载，因此 options 字段使用逗号分隔符。

> * auto / noauto: 使用 auto 选项，设备将在启动时或使用 mount -a 命令时自动挂载。auto 是默认选项。对于不能自动安装的设备，使用 noauto 选项，只能显式安装设备。
> * rw / ro: 以“读写”或“只读”模式挂载文件系统。把文件系统明确定义为 rw 可以缓解默认为只读文件系统中的一些问题，如软盘或 NTFS 分区的情况。
> * nobrowse: 作用不明，禁止磁盘在访达中显式，若删除此项，则不能开启写模式。

## 2. 开启 NTFS 硬盘的写权限

使用 root 权限在 `/etc/` 目录下创建 fstab 文件。

```shell
$ sudo vim /etc/fstab
```

以硬盘名为 `My Passport` 为例，在 fstab 文件中键入下面内容并保存。

> LABEL=My\040Passport none ntfs auto,rw,nobrowse

意思是对名为 `My Passport` 的设备，挂载到默认位置 `/Volumes/` 目录下，以 NTFS 文件系统类型，引导时自动挂载、用户可读写，永不自动备份，引导时不检查设备错误。

重新插入硬盘，就可以进行写操作了。

使用下面命令可打开 GUI 进行操作：

```shell
$ open /Volumes
```

或使用下面命令在桌面建立一个软连接（快捷方式）：

```shell
$ ln -s /Volumes/My\ Passport ~/Desktop/My\ Passport
```

## 参考链接

* [How to Enable NTFS Write Support in Mac OS X](http://osxdaily.com/2013/10/02/enable-ntfs-write-support-mac-os-x){:target="_blank"}
* [ASCII Codes - Table of ascii characters and symbols](https://ascii.cl){:target="_blank"}
