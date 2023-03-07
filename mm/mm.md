
//0. real mode
//    Address = (Segment << 4) + IP
//    A20
//
//1. protected mode
//a. GDT
//    EFLAGS PE
//    GDTR
//
//    Segment Selector(CS, DS...)
//        KERNEL_DS
//        KERNEL_CS
//        USER_DS
//        USER_DS
//
//    Address = BaseAddress + IP
//
//b. PG
//    EFLAGS PG
//    CR3
