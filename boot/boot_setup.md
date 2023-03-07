
# setup 0x90000
1. arch/x86/boot/header.S 
    0x90000
    a. bootsect
    
    0x90200
    b. setup_header

    c. start_of_setup
        初始化栈空间
        开启中断
        初始化 BSS 空间
        调用 boot/main.c 的 main 函数

            |        vmlinux         |
0x100000    +------------------------+   1M
            |          ...           |
            +------------------------+   esp(栈顶)
STACK_SIZE  |         Stack          |
009000-200  +------------------------+   heap_end_ptr(栈底/堆底)
    ?       |         Heap           |
            +------------------------+
            |          ...           |
            +------------------------+   _end
            |          BSS           |
            +------------------------+   __bss_start
            |    setup.bin other     |
0x090200    +------------------------+
            |  setup.bin bootsector  |
0x090000    +------------------------+
            |      Boot loader       |
0x007c00    +------------------------+
            |                        |
            +------------------------+

            ~                        ~   
            |  Protected-mode kernel |
100000      +------------------------+
            |  I/O memory hole       |   
0A0000      +------------------------+
            |  Reserved for BIOS     |      Leave as much as possible unused
            ~                        ~   
            |  Command line          |      (Can also be below the X+10000 mark)
X+10000     +------------------------+
            |  Stack/heap            |      For use by the kernel real-mode code.
X+08000     +------------------------+    
            |  Kernel setup          |      The kernel real-mode code.
            |  Kernel boot sector    |      The kernel legacy boot sector.
X           +------------------------+
            |  Boot loader           |      <- Boot sector entry point 0000:7C00
001000      +------------------------+
            |  Reserved for MBR/BIOS |
000800      +------------------------+
            |  Typically used by MBR |
000600      +------------------------+ 
            |  BIOS use only         |   
000000      +------------------------+


2. arch/x86/boot/main.c
    boot_params
    detect memory
    query bios
    go_to_protected_mode

3. arch/x86/boot/pm.c
    reset_coprecessor 
    mask_all_interrupts
    setup_idt
    setup_gdt

4. arch/x86/boot/pmjump.S
    jmpl    *%eax           # Jump to the 32-bit entrypoint
