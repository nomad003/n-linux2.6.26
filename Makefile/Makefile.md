arch/x86/Makefile:
    bzImage: setup.bin vmlinux.bin
    arch/x86/boot/vmlinux.bin:
        vmlinux -> arch/x86/boot/compressed/vmlinux.bin
                -> arch/x86/boot/compressed/vmlinux.bin.gz
                -> arch/x86/boot/compressed/piggy.o
                -> head_32.o + misc.o + piggy.o = arch/x86/boot/compressed/vmlinux
                -> arch/x86/boot/vmlinux.bin
    arch/x86/boot/setup.bin:
        setup-y		+= a20.o cmdline.o copy.o cpu.o cpucheck.o edd.o
        setup-y		+= header.o main.o mca.o memory.o pm.o pmjump.o
        setup-y		+= printf.o string.o tty.o video.o video-mode.o version.o
        setup-$(CONFIG_X86_APM_BOOT) += apm.o
        setup-$(CONFIG_X86_VOYAGER) += voyager.o
        setup-y		+= video-vga.o
        setup-y		+= video-vesa.o
        setup-y		+= video-bios.o

vmlinux:
    vmlinux: $(vmlinux-lds) $(vmlinux-init) $(vmlinux-main) vmlinux.o $(kallsyms.o)

    vmlinux-init := $(head-y) $(init-y)
    vmlinux-main := $(core-y) $(libs-y) $(drivers-y) $(net-y)
    vmlinux-all := $(vmlinux-init) $(vmlinux-main)
    vmlinux-lds := arch/$(SRCARCH)/kernel/vmlinux.lds

    init-y          := init/
    drivers-y       := drivers/ sound/
    net-y           := net/
    libs-y          := lib/
    core-y          := usr/
    core-y          += kernel/ mm/ fs/ ipc/ security/ crypto/ block/
    
    




