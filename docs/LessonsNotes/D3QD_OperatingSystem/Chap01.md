---

title: Chap 1 | Introduction

hide:
  #  - navigation # 显示右
  #  - toc #显示左
  #  - footer
  #  - feedback  
comments: true  #默认不开启评论

---

<h1 id="欢迎">Chap 1 | Introduction</h1>

!!! note "章节启示录"
    <!-- === "Tab 1" -->
        <!-- Markdown **content**. -->
    <!-- === "Tab 2"
        More Markdown **content**. -->
    本章节是OS的第一章。

## 1.Overview
* 什么是操作系统？    
    在计算机用户和计算机硬件之间充当媒介的程序。  
    让硬件能够更加高效地运转。
    1. resource allocator
    2. control program

![](./img/1.png){width="350"}

* 操作系统的组成（冯诺依曼架构）  
    以内存为中心   
    一个或多个cpu、设备控制器通过公共总线连接，提供对共享内存的访问    
    cpu和设备竞争内存周期的并发执行   

![](./img/2.png){width="350"}

I/O设备和CPU可以并发执行。    
每个设备控制器负责一个特定的设备类型。   
每个设备控制器都有一个本地缓冲区(local buffer)。   
CPU将数据从主存移到本地缓冲区。（外卖放到快递柜）  
I/O是从设备到控制器的本地缓冲区。(退货的货物放到快递柜)         
设备控制器通过<font color = "red">中断(interrupt)(通过系统总线via system bus)</font>通知CPU它已经完成了它的操作。   
??? question "并发(Concurrent)与并行(Parallel)"
    并发是同时开始
    并行是每个时刻两者都同时执行  
    ![](./img/3.png)


* 中断   
    中断被广泛的使用，操作系统需要处理各种中断。      
    中断通过包含所有服务例程地址的中断向量(***interrupt vector***)将控制传递给中断服务例程(ISR)。    
    中断架构(Interrupt architecture)必须保存被中断指令的地址。    
    在处理另一个中断时禁用传入中断，以防止中断丢失。 

* 中断类型   
    * 硬件中断：I/O中断   
    * 软件中断：陷阱(trap)是由错误(error)或用户请求(request)(后者通常称为系统调用)引起的软件生成的中断。  
    >注意:名称在不同的体系结构中可能会有所不同。   
    ![](./img/4.png)   

操作系统是中断驱动的。

* 中断处理   
    操作系统通过存储寄存器和程序计数器来保存CPU的状态。      
    确定发生了哪种类型的中断:   
          1. 通过泛型例程(generic routine)进行轮询(polling)   
          2. 矢量(vector)中断系统(<font color = "red">中断表</font>)     
    单独的代码段决定对每种类型的中断应该采取什么行动
    
![](./img/5.png){width="400"}


* 两种I/O的方法    
![](./img/6.png){width="500"}    
异步是非阻塞式的，不需要等待当前指令完全执行完毕（但非阻塞式不等于异步I/O处理）

* Direct Memory Access Structure    
用于能够以接近内存速度传输信息的高速I/O设备。     
设备控制器将数据块从缓冲存储器直接传输到主存储器，无需CPU干预。    
每个块(per block)只生成一个中断，而不是每个字节(per byte)生成一个中断。 

* Storage Structure
主存(Main memory)——只有CPU可以直接访问的大容量存储介质。      
辅助存储器(Secondary storage)——主存储器的扩展，提供大的非易失性存储容量。   
磁盘——覆盖有磁性记录材料的刚性金属或玻璃盘片。   
    * 磁盘表面逻辑上划分为磁道，磁道再细分为扇区。  
    * 磁盘控制器决定设备和计算机之间的逻辑交互。  

* Migration of Integer A from Disk to Register   
    多任务(Multitasking)环境必须小心使用最新的值，无论它存储在存储层次结构中的哪个位置       
    多处理器(Multiprocessor)环境必须在硬件中提供缓存一致性，以便所有cpu的缓存中都有最新的值

    ![](./img/7.png){width="500"}

    * Multiprocessor Systems   
    Each CPU processor has its own set of registers  
    All processors share physical memory over the system bus  
    ![](./img/8.png){width="300"}  

    * Multicore Systems  
    片内通信比片间通信快(在同一个L2 cache中进行会比较快，减少了数据的搬运)   
    功耗更低(适合移动设备)   
    ![](./img/9.png){width="300"}  

    * The NUMA Architecture  
    cpu之间通过共享系统互连连接  
    随着添加更多处理器，扩展更有效  
    跨互连的远程内存很慢    
    操作系统需要仔细的CPU调度和内存管理   
    ![](./img/10.png){width="300"}  

## 2.操作系统结构(分类)
* Multiprogramming needed for efficiency (CPU utilization 利用率)   
    1. 单用户无法使CPU和I/O设备始终处于繁忙状态     
    2. 多路编程组织工作(代码和数据)，所以CPU总是有一个要执行     
    3. 系统中总工作的一个子集保存在内存中   
    4. 选择一个工作并通过工作调度运行   
    5. 当它必须等待(例如I/O)时，操作系统切换到另一个工作   

* Timesharing (multitasking) is logical extension     
    分时(多任务)是一种逻辑扩展，在这种扩展中，CPU频繁地切换工作，用户可以在每个工作运行时与之交互，从而创建交互式计算(交互性)。每一个时刻只存一个用户的信息。  
    1. 响应时间应小于1秒，以便让用户之间感受不到彼此的存在  
    2. 每个用户至少有一个程序在内存中执行 => 进程  
    3. 如果多个作业准备同时运行 => CPU调度  
    4. 如果进程不适合内存，交换(swapping)将它们移进移出以运行  
    5. 虚拟内存(Virtual memory)允许执行不完全在内存中的进程   