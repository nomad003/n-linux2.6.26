
1. POST
   加电自检，CPU，计时器芯片，中断芯片，DMA控制器 

2. Initial
   枚举设备，初始化寄存器，分配中断，IO端口，DMA资源等

3. Setup
   系统设置，存于CMOS


4. 中断程序 
   INT 10h
   INT 13h
   INT 15h
   ...

5. 自举程序
   调用INT 19h，启动自举程序，读取引导记录，装载操作系统

初始化各种主板芯片组
初始化键盘控制器
初始化中断向量、中断服务例程
初始化 VGA BIOS 控制器
显示 BIOS 的版本和公司名称
扫描各种介质容量并显示
读取 CMOS 的启动顺序配置
调用 INT 0x19 启动自举程序


