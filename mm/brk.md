
mm/mmap.c
mm/memory.c
mm/page_alloc.c

1. unsigned long sys_brk(unsigned long brk) mm/mmap.c
   do_brk
   vma
   find_vma_prepare
   kmem_cache_zalloc

2. make_pages_present mm/memory.c
   get_user_pages

3. page_fault arch/x86/kernel/entry_32.S
   do_page_fault arch/x86/mm/fault.c
   handle_mm_fault
   handle_pte_fault

4. do_anonymous_page mm/memory.c
   alloc_zeroed_user_highpage_movable   

5. alloc_pages mm/page_alloc.c 
   __alloc_pages_internal
   get_page_from_freelist
   zonelist
   preferred_zone
   page = buffered_rmqueue(preferred_zone...)

 
物理内存组织 
pg_date_t(node)
    zone
        page
