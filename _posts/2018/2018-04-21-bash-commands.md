---
layout: post
title:  "UNIX/Linux 用户常用的 Bash 命令"
date:   2018-04-21 18:58:02 +0800
author: mistydew
comments: true
category: 程序人生
tags: UNIX/Linux CLI Terminal
excerpt: 一些 UNIX/Linux 下常用的 Bash 命令。
---
## 查看目录下的内容 | list

```shell
$ ls # 列出当前所在目录下的所有文件名，包含普通文件、目录文件...（不含隐藏文件，即以 . 开头的文件）
$ ls <dir> # 列出 <dir> 目录下的所有文件名，包含普通文件、目录文件...（不含隐藏文件，即以 . 开头的文件）
$ ls -l # 列出当前所在目录下的所有文件（除隐藏文件）的详细信息，分别是文件属性（类型、权限）、连接数、所属者、所属组、大小（字节）、日期、文件名。
$ ls -a # 列出当前所在目录下的所有文件名，包含隐藏文件。
$ ls * # 列出当前所在目录及其子目录下的所有文件名。
$ ls -R # 递归列出当前所在目录下的所有文件名。
```

## 切换目录 | change directory

```shell
$ cd <dir> # 切换当前所在目录至 <dir> 目录。
$ cd .. # 切换至上一级目录。
$ cd - # 切换至上一个工作目录。
$ cd [~] # 切换至当前用户家目录。
```

## 创建文件

```shell
$ touch <file> # 创建一个指定文件名为 <file> 的空文件。
$ mkdir <dir> # 创建一个指定目录名为 <dir> 的空目录。
$ mkdir -p <dir1>/<dir2> # 创建一个指定目录名为 <dir2> 的空目录，-p 参数表示若 <dir1> 不存在就创建一个。
$ echo <content> >> <file> # 追加 <content> 内容到 <file> 文件的最后一行，若文件不存在，则新建并追加。注：使用 > 代替 >>，则会覆盖现存文件当前的内容。>> 为输出重定向。
```

## 查找文件 | find

```shell
$ find . -name <file> -type f # 以当前目录 . 为起始目录，查询并显示指定文件名 <file> 的文件（相对）路径，f 表示文件类型普通文件。
$ find . -empty #  以当前目录 . 为起始目录，查询并显示所有大小为 0 的文件（相对）路径。
$ find / -name <file> -print 2>/dev/null # 以根目录 / 为起始目录，查询并显示指定文件名 <file> 的文件（相对）路径，同时把标准错误缓冲区的内容导入 /dev/null 中，以加快查找速度。
```

## 删除文件 | remove

```shell
$ rm <file> # 删除指定的 <file> 文件。
$ rmdir <dir> # 只能删除指定的空目录 <dir>。
$ rm <dir> -rf # 删除指定的目录 <dir> 下包含的所有文件，-r 参数表示循环，-f 参数表示不显示任何信息。
$ find . -name <file> | xargs rm # 批量删除当前目录下所有 <file> 文件，xargs 参数是一个过滤器，把参数列表分段传递给另一个命令，与管道 | 一起使用。
```

**注：慎用 "$ sudo rm / -rf"，详见[多一个空格会发生什么？](https://github.com/MrMEEE/bumblebee-Old-and-abbandoned/commit/6cd6b2485668e8a87485cb34ca8a0a937e73f16d){:target="_blank"}**

## 搜索内容 | grep

```shell
$ grep <content> * -nri # 查询并显示当前所在目录下所有出现 <content> 字符串的文件及行号，* 表示当前目录下所有文件，-n 参数显示所在行号，-r 参数表示递归，-i 参数表示不区分大小写。
$ grep <content> <file> -n # 查询并显示指定文件 <file> 出现 <content> 字符串的行号。
```

## 修改文件名 | rename

```shell
$ find . -exec rename 's/<from>/<to>/' {} ";" # 批量修改当前目录下的文件名中字符串 <from> 为 <to>。
```

## 字符串替换 | sed

```shell
$ sed -i 's/<from>/<to>/g' <file> # 把文件 <file> 中的字符串 <from> 全部改为 <to>。
$ sed -i '' 's/<from>/<to>/g' # macOS 下指定一个字符串作为备份文件后缀，若字符串为空，则不备份。
$ sed -i 's/\t/    /g' <file> # 把文件 <file> 中的 tab 全部改为 4 个空格。
$ sed -i '/<content>/,+1d' <file> # 把文件 <file> 中的含 <content> 的行及其下一行（,+1）移除。
$ sed -i '/<dist>/i\<new>' <file> # 在文件 <file> 中含字符串 <dist> 的行上面新增一行内容 <new>。
$ sed -i '/<dist>/a\<new>' <file> # 在文件 <file> 中含字符串 <dist> 的行下面新增一行内容 <new>。
$ find <dir> -type f -print0 | xargs -0 sed -i 's/<from>/<to>/g' # 把 <dir> 下全部文件中的字符串 <from> 全部改为 <to>。
```

## 查看文件 | cat

```shell
$ cat -A <file> # 把文件 <file> 内容全部输出至标准输出，-A 用于显示不可打印字符，行尾换行显示为 $。
$ more <file> # 可滚动翻看文件 <file> 的内容，Space 键查看下一屏，Enter 键查看下一行，B 键查看上一屏。
```

## 统计文件内容 | wc

```shell
$ wc -l <file> # 统计文件 <file> 的行数。参数 -c 统计字符数即文件大小字节数，-w 统计字数。
```

## 统计文件个数 | ls | wc

```shell
$ ls -l | grep "^-" | wc -l # 统计当前目录下普通文件个数。
$ ls -l | grep "^d" | wc -l # 统计当前目录下目录文件个数。
$ ls -lR | grep "^-" | wc -l # 统计当前目录及其子目录下普通文件个数。
$ ls -lR | grep "^d" | wc -l # 统计当前目录及其子目录下目录文件个数。
```

## 创建链接文件 | link

```shell
$ ln <srcfile> <destfile> # 创建源文件 <srcfile> 的硬链接 <destfile>，删除源文件不影响其硬链接文件。<br>
$ ln -s <srcfile> <destfile> # 创建源文件 <srcfile> 的软链接 <destfile>，类似于 Windows 下的快捷方式，删除源文件后其软链接文件失效。
```

## 解压缩文件 | tar

```shell
$ tar zcfv <file>.tar.gz <file> # 压缩文件 <file> 为 gz 格式 <file>.tar.gz。
$ tar xfv <file>.tar.gz # 解压缩 gz 格式的文件 <file>.tar.gz。
$ tar Jcfv <file>.tar.xz <file> # 压缩文件 <file> 为 xz 格式 <file>.tar.xz。
$ tar Jxfv <file>.tar.xz # 解压缩 xz 格式的文件 <file>.tar.xz。
```

## 安装软件 | install

```shell
$ sudo apt install <software> # 安装 <software> 到系统，一般默认装在 /usr/bin 目录下，所以要使用 root 用户进行安装，需使用 sudo 进行权限提升。
$ sudo apt update # 从所有配置的源中下载包信息，用于安装软件时显示没有该软件的情况。
$ sudo apt upgrade # 从通过 sources.list 配置的源安装在系统上的所有包中安装可用的升级，即通过 update 的结果进行已安装软件的升级。
```

## 域名信息查询 | domain name system lookup

```shell
$ nslookup <url> # 查询指定域名 <url> 的真正域名和对应 IP，<url> 可能是别名，例：百度。
```

## 添加/删除用户 | useradd/userdel

```shell
$ sudo useradd -m <user> -s /bin/bash # 添加一个用户 <user>，-m 参数为该用户在 /home 目录下创建同名目录，-s 参数为该用户指定 bash 脚本编译器。
$ sudo userdel -d <user> -s -g # 删除用户 <user>（不会删除用户目录），-d 删除用户家目录，-s 使用的 bash，-g 用户所属组。
$ sudo cat /etc/passwd # 查看用户信息。
$ sudo cat /etc/shadow # 查看用户（加密的）密码。
```

## 修改用户密码 | passwd

```shell
$ sudo passwd [root] # 初始化 root 用户密码。
$ passwd [current_user] # 修改当前用户密码。
$ sudo passwd [other_user] # 修改其他用户密码。
```

## 设置超级用户 sudoers | visudo

```shell
$ sudo visudo # 打开 /etc/sudoers.tmp 文件。
```

在 root ALL=(ALL:ALL) ALL 下添加 sudoers。

> \# User privilege specification<br>
> root ALL=(ALL:ALL) ALL<br>
> user ALL=(ALL:ALL) ALL

## 添加/删除用户组 | group add/delete

```shell
$ sudo groupadd <groupname> # 添加用户组 <groupname>。
$ sudo groupdel <groupname> # 删除用户组 <groupname>（不能为主组）。
$ sudo usermod -G <groupname> <username> # 把用户 <username> 添加到组 <groupname> 中，-g 修改用户的组。
$ sudo cat /etc/group # 查看组信息。
```

## 更改文件所属用户（或组）| change owner

```shell
$ chown <username> <file> # 改变文件 <file> 所属用户名 <username>。
$ chown <username>:<group> <file> # 改变文件 <file> 所属用户名 <username> 或组 <group>。
```

**注：只有文件创建者和 root 用户才能使用此命令，且 `<username>` 必须是本机存在的用户名。本机用户名记录在 "/etc/passwd" 文件中。**

## 更改文件所属组 | change group

```shell
$ chgrp <group> <file> # 改变文件 <file> 所属组 <group>。
```

**注：只有文件创建者和 root 用户才能使用此命令，且 `<group>` 必须是本机存在的组。本机用户所属组记录在 "/etc/group" 文件中。**

## 进程的前后台切换 | & jobs fg Ctrl+Z bg

```shell
$ ./<ELF> & # 后加 & 符号可把程序 <ELF> 放后台运行。
$ jobs # 可查看后台运行的程序，包含 Ctrl+Z 暂停的进程，及其序号和运行状态。
$ fg n # 把 jobs 列出的后台序号为 n 的任务拉到前台运行。
$ Ctrl + Z # 把当前前台运行的程序放到后台并挂起。
$ bg n # 把 jobs 列出的后台序号为 n 的任务继续运行。
```

## 列出打开文件的进程 | list open file

```shell
$ lsof -i [:<port>] # 列出占用端口 <port> 的进程，-i 用于指定条件。
$ lsof -i tcp/udp[:<port>] # 列出占用 tcp/udp 端口 <port> 的进程。
```

## 发信号到进程 | kill

```shell
$ kill -l # 列出全部信号。
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
$ kill -9 <pid> # 杀死指定 <pid> 的进程。
$ kill -19 <pid> # 暂停指定进程 <pid>，和 Ctrl+Z 效果相同。
```

## 统计时间 | time

```shell
$ time <command> # 统计执行给定命令 <command> 所花费的时间。
```

## 修改时区 | time zone

```shell
$ timedatectl status # 查看系统时间和日期。
$ timedatectl set-timezone "Asia/Shanghai" # 修改系统时区为中国上海。
$ timedatectl set-local-rtc 1 # 把实时时钟设置为本地时间，0 表示设置为 UTC，适用于 Win/Linux 双系统。
```

## 查看磁盘

```shell
$ du -sh # 查看当前所在目录下各文件的总大小，参数 -s 同 --summarize 表示总和，参数 -h 同 --human-readable 表示以当前大小可表示的最大单位来显示信息，若不加该参数，则单位默认为 KB。
$ du -h -d 1 # 查看当前所在目录下深度为 1 的各文件的总大小，参数 -d N 同 --max-depth=N 表示显示的目录层级的最大深度为 N。
$ df -h # 查看当前磁盘各分区的使用情况，参数 -h 同 --human-readable 含义同上。
$ fdisk -l # 查看当前磁盘及其分区的详细信息。注：若不显示任何信息，命令前需加 sudo 来提升权限。
```

## 显示登陆用户 | w

```shell
$ w # 显示当前登陆的用户及其正在执行的命令。
```

## 查看内核参数 | sysctl

```shell
$ sysctl -a # 查看 Linux 系统内核的全部参数。
```

## 查看系统版本信息 | release

```shell
$ uname -a # 打印当前系统相关信息，-a 表示全部信息。
$ lsb_release -a # 显示 LSB 和版本信息，-a 表示全部信息。
$ cat /etc/issue # 查看系统版本信息。
```

## 关机 | shutdown

```shell
$ shutdown -h now # 立刻关机，h 表示 halt，可以指定时间。
$ halt # 同 shutdown -h now。
```

## 远程安全连接 | SSH (Secure Shell)

```shell
$ sudo apt install ssh # 安装 ssh 服务。
$ ps -elf | grep ssh # 看到 /usr/sbin/sshd -D 的进程表示 ssh 服务启动正常。
$ ssh <username>@<hostname> # 连接至指定“用户名@主机 IP”的主机上，根据提示输入相应密码。
```

## 远程安全拷贝 | secure copy

```shell
$ scp <file> username@ip:<dir> # 复制本地文件 <file> 到远程主机 username@ip 的 <dir> 目录下。
$ scp username@ip:<file> <dir> # 复制远程主机 username@ip 下的文件 <file> 到本地 <dir> 目录下。
```

**注：username 和 ip 的顺序，且需要知道 username 对应的密码。**

## 精简可执行文件 | strip

```shell
$ strip <ELF> # 通过删除可执行文件 <ELF> 中的调试符号等相关信息减少其大小，此操作不可逆。
```

## 参考链接

* [Why Penguin is Linux logo? - LinuxScrew: Linux Blog](http://www.linuxscrew.com/2007/11/14/why-penguin-is-linux-logo){:target="_blank"}
* [The Linux Kernel Archives](https://www.kernel.org){:target="_blank"}