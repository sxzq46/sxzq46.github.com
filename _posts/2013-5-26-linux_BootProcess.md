---
layout : post
category : lessons
tags : [开始]
---
linux引导流程

	固件firmware （CMOS/BIOS） POST加电自检-硬盘MBR 软硬件时钟设置查看同步

	自举程序BootLoader （GRUB） 载入内核  单用户模式、光盘修复模式 *  GRUB命令操作、GRUB密码设置 /boot/grub/grub.conf

				内核  a.驱动硬件  内核保存目录、内核版本 dmesg

			init  PID=1 内核调度器PID=0 父子进程（孤儿进程、僵尸进程）
Linux中如果一个进程的父进程结束，而该进程由于某种原因没有结束，则被系统发现后自动将该进程指向init进程。所以理论上init进程是所有进程的父进程。
如果子进程结束，而父进程不知。则该子进程编程僵尸进程。

					/etc/inittab  文件格式  运行级别

						initdefault

				/etc/rc.d/init.d/rc.sysinit  系统初始化脚本

						/etc/rc.d/rc

/etc/rc.d/rcN.d  如何更改自启动程序*  ln -s、chkconfig、ntsysv 手动启动服务* /etc/rc.d/init.d /var/log/messages

					/etc/X11/prefdm 启动X window

						/etc/passwd
两个action，一个wait表示这个命令启动完之后才继续开启其他命令
如果一个命令是开机自启的，那么它的action就是wait
powerfail：当出现电源错误时执行process指定的命令，不等待其结束
powerokwait：当店员恢复时执行process指定的命令
respawn：一旦process指定的命令终止，便重新运行该命令

						/etc/shadow