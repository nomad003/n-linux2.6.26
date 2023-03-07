
arch/x86/boot/gzImage:

            -------------------------
            |       setup.bin       |
        ->  ------------------------- _head (startup_32)
            |.head.text             |
            |                       |               uncompressed code(arch/x86/boot/compressed/head_32.S:ENTRY(startup_32))
            |                       |
            |                       |
vmlinux.bin |                       |
            -------------------------
            |                       |
            |   .rodata..compressed |               compressed code (vmlinux.bin.gz)
            |                       |
            ------------------------- _text
            |.text                  |
            ------------------------- _rodata       uncompressed code (part of head_32.o, misc.o, string.o ... )
            |.rodata                |
            ------------------------- _got
            |.got                   |
            ------------------------- _data
            |.data                  |
        ->  ------------------------- _bss
            |                       |
            ------------------------- _ebss
            |                       |
            ------------------------- _end

linux-2.6.26/arch/x86/boot
[chenyao@localhost boot]$ tree
.
├── a20.c
├── apm.c
├── bitops.h
├── boot.h
├── cmdline.c
├── code16gcc.h
├── compressed
│  ├── head_32.S
│  ├── head_64.S
│  ├── Makefile
│  ├── misc.c
│  ├── relocs.c
│  ├── vmlinux_32.lds
│  ├── vmlinux_64.lds
│  └── vmlinux.scr
├── copy.S
├── cpu.c
├── cpucheck.c
├── edd.c
├── header.S
├── install.sh
├── main.c
├── Makefile
├── mca.c
├── memory.c
├── mkcpustr.c
├── mtools.conf.in
├── pm.c
├── pmjump.S
├── printf.c
├── setup.ld
├── string.c
├── tools
│  └── build.c
├── tty.c
├── version.c
├── vesa.h
├── video-bios.c
├── video.c
├── video.h
├── video-mode.c
├── video-vesa.c
├── video-vga.c
└── voyager.c

2 directories, 42 files

#arch/x86/boot/Makefile:

targets         := vmlinux.bin setup.bin setup.elf zImage bzImage
subdir-         := compressed

setup-y         += a20.o cmdline.o copy.o cpu.o cpucheck.o edd.o
setup-y         += header.o main.o mca.o memory.o pm.o pmjump.o
setup-y         += printf.o string.o tty.o video.o video-mode.o version.o

$(obj)/bzImage: IMAGE_OFFSET := 0x100000
$(obj)/zImage $(obj)/bzImage: $(obj)/setup.bin \
                              $(obj)/vmlinux.bin $(obj)/tools/build FORCE
        $(call if_changed,image)
        @echo 'Kernel: $@ is ready' ' (#'`cat .version`')'

OBJCOPYFLAGS_vmlinux.bin := -O binary -R .note -R .comment -S

#arch/x86/boot/compressed/Makefile:

├── compressed
│  ├── head_32.S
│  ├── head_64.S
│  ├── Makefile
│  ├── misc.c
│  ├── relocs.c
│  ├── vmlinux_32.lds
│  ├── vmlinux_64.lds
│  └── vmlinux.scr

targets := vmlinux vmlinux.bin vmlinux.bin.gz head_$(BITS).o misc.o piggy.o

#Makefile:

[chenyao@localhost linux-2.6.26]$ tree -L 1
.
├── arch
├── block
├── crypto
├── drivers
├── fs
├── include
├── init
├── ipc
├── kernel
├── lib
├── Makefile
├── mm
├── net
├── security
├── sound
├── usr
└── virt

linux-2.6.26/arch/x86/
├── boot
├── crypto
├── kernel
├── lib
├── mach-default
├── mach-generic
├── mm
├── oprofile
├── pci
├── power
├── vdso

.vmlinux.cmd
md_vmlinux := 
ld -m elf_i386 --build-id -o vmlinux -T arch/x86/kernel/vmlinux.lds 
arch/x86/kernel/head_32.o arch/x86/kernel/head32.o arch/x86/kernel/init_task.o  
init/built-in.o 
--start-group  
arch/x86/mach-generic/built-in.o 
arch/x86/mach-default/built-in.o  
arch/x86/kernel/built-in.o  
arch/x86/mm/built-in.o  
arch/x86/crypto/built-in.o  
arch/x86/vdso/built-in.o  
arch/x86/pci/built-in.o  
arch/x86/oprofile/built-in.o  
arch/x86/power/built-in.o  
arch/x86/lib/built-in.o  
arch/x86/lib/lib.a  
kernel/built-in.o  
usr/built-in.o  
mm/built-in.o  
fs/built-in.o  
ipc/built-in.o  
security/built-in.o  
crypto/built-in.o 
block/built-in.o  
drivers/built-in.o  
sound/built -in.o  
net/built-in.o 
lib/built-in.o  
lib/lib.a  
--end-group 
.tmp_kallsyms2.o
