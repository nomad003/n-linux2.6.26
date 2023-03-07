
# kernel decompress 0x100000(1M)
1. arch/x86/boot/compressed/head_32.S    
    a. startup_32

    b. relocated

2. arch/x86/boot/compressed/misc.c
    a. decompress_kernel

# kernel init 0x100000
1. arch/x86/kernel/head_32.S
    a. startup_32
    b. default_entry
        0 - INIT_MAP_BEYOND_END
        10处是建立PDE项，11是建立PTE项。此处会建立从0~INIT_MAP_BEYOND_END的对等映射，且虚拟地址__PAGE_OFFSET（3G）的目录项共享从0开始的虚拟地址的PTE。因此3G~3G+INIT_MAP_BEYOND_END映射到物理地址0~INIT_MAP_BEYOND_END。
        注意因为链接时vmlinux起始地址为3G+1M，而物理地址为1M，因此要用pa宏来引用物理地址。
    c. setup_idt

2. arch/x86/kernel/head32.c
    a. i386_start_kernel
        start_kernel

3. init/main.c
    a. start_kernel
