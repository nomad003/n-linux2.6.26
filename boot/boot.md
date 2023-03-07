0. BIOS
    load bootloader
    boot device, first sector, 512bit, 0x7c00

1. bootloader(GRUB)
    load kernel

	|			                 |
0A0000	+------------------------+
	|  Reserved for BIOS	     |	Do not use.  Reserved for BIOS EBDA.
09A000	+------------------------+
	|  Command line		         |
	|  Stack/heap		         |	For use by the kernel real-mode code.
098000	+------------------------+	
	|  Kernel setup		         |	The kernel real-mode code.
090200	+------------------------+
	|  Kernel boot sector	     |	The kernel legacy boot sector.
090000	+------------------------+
	|  Protected-mode kernel     |	The bulk of the kernel image.
010000	+------------------------+
	|  Boot loader		         |	<- Boot sector entry point 0000:7C00
001000	+------------------------+
	|  Reserved for MBR/BIOS     |
000800	+------------------------+
	|  Typically used by MBR     |
000600	+------------------------+ 
	|  BIOS use only	         |
000000	+------------------------+


2. kernel

3. rootfs
