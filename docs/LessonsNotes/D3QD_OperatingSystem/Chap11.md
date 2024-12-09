---

title: Chap 11 | File System Implementation

hide:
  #  - navigation # 显示右
  #  - toc #显示左
  #  - footer
  #  - feedback  
comments: true  #默认不开启评论

---

<h1 id="欢迎">Chap 11 | File System Implementation</h1>

!!! note "章节启示录"
    <!-- === "Tab 1" -->
        <!-- Markdown **content**. -->
    <!-- === "Tab 2"
        More Markdown **content**. -->
    本章节是OS的第十一章。

## 1.File-System Structure and Implementation
* Layered File System：     
    ![](./img/103.png)

    1. I/O控制层：包括设备驱动程序和终端处理程序，以在主内存和磁盘系统之间传输信息。设备驱动程序可以作为翻译器。
    2. 基本文件系统：只需向适当设备驱动程序发送通用命令，以读取和写入磁盘的物理块。每个物理块由磁盘的数字地址来标识。
    3. 文件组织模块：知道文件及其逻辑块以及物理块。可以将逻辑块地址转成物理块地址，以供基本文件系统传输。
    4. 逻辑文件系统：管理元数据信息。元数据包括文件系统的所有结构，而不包括实际数据（或文件内容）。

    >


    * 分层是为了更好地模块化，把特定功能封装起来，使得用户只需要从逻辑层面关心操作。
    * i-node在logical这个层面进行管理

* Disk structures：
    1. Boot control block (per volume 每个卷的) 引导控制块，可以包含从该卷引导操作系统的所需信息。
    2. Volume control block per volume (superblock in Unix 每个卷的) 超级块，存整个卷的metadata，包括卷（或分区）的详细信息（如分区的块的数量、块的大小、空闲块的数量和指针、空闲的FCB数量和FCB指针等）
    3. Directory structure per file system（每个文件系统的） 
    4. Per-file FCB (inode in Unix)

* In-Memory File System Structures：    
    ![](./img/104.png){width="400"}

    * Figure 12-3(a) refers to opening a file.寻找到对应的文件，在内存中建立特定的结构
    
    * Figure 12-3(b) refers to reading a file.有两个打开文件表（上一章提到的），如果在系统层面的打开文件表已经打开，可以直接建立映射，并直接阅读到data block的特定位置。
    
    * 更直观的图：分两个阶段去做，通过FCB可以直接访问到文件A      
    ![](./img/105.png){width="500"}

* Virtual File Systems：VFS提供两个重要功能：
    1. 通过定义一个清晰的VFS接口，它将文件系统的通用操作和实现分开。VFS接口的多个实现可以共存在同一台机器上，允许透明访问本地安装的不同类型的文件系统。
    2. 使用vnode唯一表示网络上的一个文件。这种网络的唯一性需要用来支持网络文件系统。内核为每个活动节点（文件或目录）保存一个vnode结构。
    

    ![](./img/106.png){width="400"}

* In-Memory VFS Objects：
    1. superblock object: a specific mounted filesystem, corresponding to (but not equal) the superblock in the disk structure 表示整个文件系统
    2. inode object: a specific file, corresponding to (but not equal) FCB in the disk structure 表示一个单独的文件
    3. dentry object: an individual directory entry 表示单个目录条目 
    4. file object: an open file as associated with a process, existing as long as the file is opened 表示一个已打开的文件
    


## 2.Directory Implementation

## 3.Allocation Methods

## 4.Free-Space Management 

## 5.Efficiency and Performance
