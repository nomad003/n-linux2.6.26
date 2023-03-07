1. BIOS
	a. 加电自检 系统配置 BIOS中断
	b. RESET CPU实模式,CS=0xffff,IP=0x0,第一条指令0xffff0
	c. 找到可启动设备,例:硬盘引导扇区

	0xFFFE_0000 - 0xFFFF_FFFF: 128 kilobyte ROM mapped into address space

2. 引导程序
	a. 第一个扇区512字节,结尾0xAA55
			第一条指令内存地址0x7c00
			0xA0000(640K)以上用于图形界面及BIOS本身
			包含简单文件系统和设备驱动程序,可以读取操作系统镜像
	0x00000000 - 0x000003FF - Real Mode Interrupt Vector Table
	0x00000400 - 0x000004FF - BIOS Data Area
	0x00000500 - 0x00007BFF - Unused
	0x00007C00 - 0x00007DFF - Our Bootloader
	0x00007E00 - 0x0009FFFF - Unused
	0x000A0000 - 0x000BFFFF - Video RAM (VRAM) Memory
	0x000B0000 - 0x000B7777 - Monochrome Video Memory
	0x000B8000 - 0x000BFFFF - Color Video Memory
	0x000C0000 - 0x000C7FFF - Video ROM BIOS
	0x000C8000 - 0x000EFFFF - BIOS Shadow Area
	0x000F0000 - 0x000FFFFF - System BIOS

	c. arch/i386/boot
			bootsect.S Linux引导扇区的源代码,汇编后不超过512字节
				move itself to 0x90000
				loads setup after itself 0x90200(使用BIOS中断int 0x13读取磁盘)
				loads system at 0x10000
			setup.S
				保护模式
				解压缩
					compressed 解压缩内核镜像
					head.S
					misc.c
					lib/inflate.c
			video.S

	d. 引导扇区镜像bootsect
	   辅助程序setup 
       内核本身镜像vmlinux

	e. 小于508KB zImage 大于 bzImage
	   解压后都是在地址0x100000(1MB)
	
	         | Protected-mode kernel  |
	100000   +------------------------+
	         | I/O memory hole        |
	0A0000   +------------------------+
	         | Reserved for BIOS      | Leave as much as possible unused
	         ~                        ~
	         | Command line           | (Can also be below the X+10000 mark)
	X+10000  +------------------------+
	         | Stack/heap             | For use by the kernel real-mode code.
	X+08000  +------------------------+
	         | Kernel setup           | The kernel real-mode code.
	         | Kernel boot sector     | The kernel legacy boot sector.
	       X +------------------------+
	         | Boot loader            | <- Boot sector entry point 0x7C00
	001000   +------------------------+
	         | Reserved for MBR/BIOS  |
	000800   +------------------------+
	         | Typically used by MBR  |
	000600   +------------------------+
	         | BIOS use only          |
	000000   +------------------------+

3. 内核初始化
	a. CPU本身初始化
	   		arch/i386/kernel/head.S	
	   		startup_32 0x100000(虚拟地址0xC0100000 3GB+0x100000)

			寻址时,才会启动段页查找,正常IP自增,不需要!!!

			页式映射的建立

	b. 内存管理和进程管理
			init/main.c
			start_kernel

	c. 设备初始化
