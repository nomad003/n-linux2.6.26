
arch/x86/kernel/e820.c

arch/x86/mm/
├── discontig_32.c
├── dump_pagetables.c
├── extable.c
├── fault.c
├── highmem_32.c
├── hugetlbpage.c
├── init_32.c
├── init_64.c
├── ioremap.c
├── k8topology_64.c
├── Makefile
├── mmap.c
├── numa_64.c
├── pageattr.c
├── pageattr-test.c
├── pat.c
├── pgtable_32.c
├── pgtable.c
└── srat_64.c

mm
├── allocpercpu.c
├── backing-dev.c
├── bootmem.c
├── bounce.c
├── dmapool.c
├── fadvise.c
├── filemap.c
├── filemap_xip.c
├── fremap.c
├── highmem.c
├── hugetlb.c
├── internal.h
├── Kconfig
├── maccess.c
├── madvise.c
├── Makefile
├── memcontrol.c
├── memory.c
├── memory_hotplug.c
├── mempolicy.c
├── mempool.c
├── migrate.c
├── mincore.c
├── mlock.c
├── mmap.c
├── mmzone.c
├── mprotect.c
├── mremap.c
├── msync.c
├── nommu.c
├── oom_kill.c
├── page_alloc.c
├── page_io.c
├── page_isolation.c
├── pagewalk.c
├── page-writeback.c
├── pdflush.c
├── prio_tree.c
├── quicklist.c
├── readahead.c
├── rmap.c
├── shmem_acl.c
├── shmem.c
├── slab.c
├── slob.c
├── slub.c
├── sparse.c
├── sparse-vmemmap.c
├── swap.c
├── swapfile.c
├── swap_state.c
├── thrash.c
├── tiny-shmem.c
├── truncate.c
├── util.c
├── vmalloc.c
├── vmscan.c
└── vmstat.c

0. 内存条
    内存条 DIMM针脚插槽类型
    BIOS设置不同针脚的高低电平，内存反馈其规格信息给BIOS，BIOS计算出容量

1. 探测物理内存布局 e820
    BIOS提供接口，0x15中断，ax寄存器设置e820
    /arch/x86/boot/memory.c
    detect_memory(void) detect_memory_e820(void)
    intcall(0x15, &ireg, &oreg)
    ireg.ax = e820
    ireg.di = (size_t)&buf
    struct boot_e820_entry{
        __u64 addr;
        __u64 size;
        __u32 type;
    }__attribute__((packed));

    boot_params.e820_table
    {{addr=0, size=654336, type=1},
     {addr=654336, size=1024, type=2},
     ...
    } 

    /proc/iomem

    e820__memory_setup
    e820__print_table(dmesg)

2. memblock 

3. Buddy    

