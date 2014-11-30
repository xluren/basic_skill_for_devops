####some procs about the info of  the hardware 

	查看CPU信息：cat /proc/cpuinfo

	查看板卡信息：cat /proc/pci

	查看PCI信息：lspci (相比cat /proc/pci更直观）

	查看内存信息：cat /proc/meminfo

	查看USB设备：cat /proc/bus/usb/devices

	查看键盘和鼠标:cat /proc/bus/input/devices

	查看系统硬盘信息和使用情况：fdisk & disk – l & df

	查看各设备的中断请求(IRQ):cat /proc/interrupts

	查看启动硬件检测信息日志：dmesg more /var/log/dmesg
	uname -a

	uptime

	几种查看Linux版本信息的方法：

	1. uname -a

	2. cat /proc/version

	3. cat /etc/issue

	4. lsb_release -a

	5.cat /etc/redhat-release

	6.rpm -q redhat-release

####how to  check  the hardware information of the  linux server

	1.显卡信息
	　　dmesg | grep -i vga 
	　　lspci | grep -i vga //查看显卡信息

	2.dmidecode | grep -i 'serrial number' //查看主板信息，查看主板的序列号

	3.CPU信息

	　　#通过/proc文件系统
	　　cat /proc/cpuinfo
