
memblock:
struct memblock {
    memory
    reserved
}
struct memblock_type {
    cnt
    max
    size
    regions
    name
}
struct memblock_region[] {
    base
    size
    flag
}


startup_32(arch/x86/kernel/head_32.S)

  -> i386_start_kernel(arch/x86/kernel/head32.c)

        ->start_kernel(init/main.c)

           ->  setup_arch(arch/x86/kernel/setup_32.c)

              ->  setup_memory(arch/x86/kernel/e820.c)

