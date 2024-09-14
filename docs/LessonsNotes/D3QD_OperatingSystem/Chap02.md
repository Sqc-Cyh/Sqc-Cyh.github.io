---

title: Chap 2 | Operating-System Structures

hide:
  #  - navigation # 显示右
  #  - toc #显示左
  #  - footer
  #  - feedback  
comments: true  #默认不开启评论

---

<h1 id="欢迎">Chap 2 | Operating-System Structures</h1>

!!! note "章节启示录"
    <!-- === "Tab 1" -->
        <!-- Markdown **content**. -->
    <!-- === "Tab 2"
        More Markdown **content**. -->
    本章节是OS的第二章。

## 1.Operating System Services
1. 用户界面(User interface):
    几乎所有的操作系统都有一个用户界面(UI)   
    不同于命令行(CLI)、图形用户界面(GUI)、批处理
2. 程序执行(Program execution):
    系统必须能够将程序装入内存并运行该程序，结束执行，正常或异常(指示错误)。
3. I/O操作:
    运行的程序可能需要I/O，这可能涉及文件或I/O设备。
4. 文件系统操作(File-system manipulation):
    程序需要读取和写入文件和目录，创建和删除它们，搜索它们，列出文件信息，权限管理。
5. 通信:   
    进程可以在同一台计算机上或通过网络在计算机之间交换信息    
    通信可以通过共享内存或通过消息传递(由操作系统移动的数据包)进行。
6. 错误检测   
7. 资源分配   
8. 审计accounting/logging:     
    跟踪哪些用户使用了多少和哪种计算机资源  
9. 保护和安全    
    


## 2.System Calls
主要由程序通过 Application Program Interface(API) 访问，而不是直接使用系统调用  