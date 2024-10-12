---

title: Chap 5 | CPU Scheduling

hide:
  #  - navigation # 显示右
  #  - toc #显示左
  #  - footer
  #  - feedback  
comments: true  #默认不开启评论

---

<h1 id="欢迎">Chap 5 | CPU Scheduling</h1>

!!! note "章节启示录"
    <!-- === "Tab 1" -->
        <!-- Markdown **content**. -->
    <!-- === "Tab 2"
        More Markdown **content**. -->
    本章节是OS的第五章。

## 1.基础概念
为什么要进行CPU调度？通过 multiprogramming 获得的最大CPU利用率


从内存中准备好的进程中进行选择执行，并将CPU分配给其中一个

* CPU调度决策可能发生在进程:
    1. 从运行状态切换到等待状态
    2. 从运行状态切换到就绪状态
    3. 从等待切换到准备
    4. 终止     

    1和4下的调度是非抢占式的（nonpreemptive）        
    所有其他调度都是抢占式的（preemptive）

* Dispatcher模块将CPU的控制权交给进程，并由短期调度器选择:
    1. 切换上下文
    2. 切换到用户模式（不一定需要切换到用户态，可能仍在内核态运行）
    3. 跳转到用户程序中的适当位置
    4. 重新启动程序

    调度延迟：调度程序停止一个进程并启动另一个进程所需的时间

## 2.Scheduling Criteria
* CPU利用率：使CPU尽可能的繁忙
* 吞吐量：每单位时间内完成执行的进程数
* 周转时间：执行一个特定过程的时间
* 等待时间：进程在就绪队列（ready queue）中等待的时间
* 响应时间：从请求被提交到产生第一个响应所花费的时间，而不是输出(对于分时环境)