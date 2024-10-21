---

title: Chap 6 | Process Synchronization

hide:
  #  - navigation # 显示右
  #  - toc #显示左
  #  - footer
  #  - feedback  
comments: true  #默认不开启评论

---

<h1 id="欢迎">Chap 6 | Process Synchronization</h1>

!!! note "章节启示录"
    <!-- === "Tab 1" -->
        <!-- Markdown **content**. -->
    <!-- === "Tab 2"
        More Markdown **content**. -->
    本章节是OS的第六章。

## 1.Background
进程同步是指在多进程或多线程环境中，为了确保多个进程或线程能够按照一定的顺序或条件正确地访问共享资源而采取的各种协调机制。

* 使用count来追踪整个buffer
    1. 生产者生产后count++
    2. 消费者消费后count--

```c++
while(true){
    while(count == BUFFER_SIZE);
    buffer[in] = nextProduced;
    in = (in + 1) % BUFFER_SIZE;
    count++;
}
```
```c++
while(true){
    while(count == 0);
    nextConsumed = buffer[out];
    out = (out + 1) % BUFFER_SIZE;
    count--;
}
```

### Race Condition （竞态条件）:
竞态条件是指一个内存位置被并发访问，并且至少有一次是写访问。

* count++ could be implemented as
    1. register1 = count     
    2. register1 = register1 + 1     
    3. count = register1

* count-- could be implemented as
    1. register2 = count     
    2. register2 = register2 - 1     
    3. count = register2

如果在抢占式中打乱顺序就会出错    
![](./img/45.png){width="500"}


??? question "对于访问共享的内核数据(shared kernel data)，非抢占的内核是否受竞态条件(race conditions)的影响？"
    多核情况下，两个进程同时运行时还是有可能出现竞态条件。

* 临界资源：指的是在多线程或多进程环境中被多个线程或进程共享的资源。这些资源需要通过适当的同步机制来保护，以确保同一时刻只有一个线程或进程可以访问这些资源。        
  
* 临界区(Critical section)：指的是程序中一个访问共用资源的程序片段。每个进程中访问临界资源的那段代码称为临界区。

![](./img/46.png)