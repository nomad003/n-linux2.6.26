0. BIOS
    load bootloader
    boot device, first sector, 512bit, 0x7c00

1. bootloader(GRUB)
    load kernel

    0x090000 boot sector
    0x090200 setup

    0x0100000 kernel image

        |  Protected-mode kernel |
100000  +------------------------+
	    |  I/O memory hole	     |
0A0000	+------------------------+
	    |  Reserved for BIOS	 |	Leave as much as possible unused
	    ~                        ~
	    |  Command line		     |	(Can also be below the X+10000 mark)
X+10000	+------------------------+
	    |  Stack/heap		     |	For use by the kernel real-mode code.
X+08000	+------------------------+	
	    |  Kernel setup		     |	The kernel real-mode code.
	    |  Kernel boot sector	 |	The kernel legacy boot sector.
X       +------------------------+  <- 0x090000
	    |  Boot loader		     |	<- Boot sector entry point 0000:7C00
001000	+------------------------+
	    |  Reserved for MBR/BIOS |
000800	+------------------------+
	    |  Typically used by MBR |
000600	+------------------------+ 
	    |  BIOS use only	     |
000000	+------------------------+

3. setup
    _start                          arch/x86/boot/header.S      
    main                            arch/x86/boot/main.c
    go_to_protected_mode            arch/x86/boot/pm.c
    protected_mode_jump             arch/x86/boot/pmjump.S

    startup_32                      arch/x86/boot/compressed/head_32.S 0x100000(1M)
    decompress_kernel               arch/x86/boot/compressed/misc.c

    startup_32                      arch/x86/kernel/head_32.S
    ...
    i386_start_kernel               arch/x86/kernel/head32.c

    start_kernel                    init/main.c
     
4. init

5. kernel 

6. rootfs
