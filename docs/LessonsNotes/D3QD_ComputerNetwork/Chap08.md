---

title: Chap 8 | 网络安全

hide:
  #  - navigation # 显示右
  #  - toc #显示左
  #  - footer
  #  - feedback  
comments: true  #默认不开启评论

---
<h1 id="欢迎">Chap 8 | 网络安全</h1>
!!! note "章节启示录"
    <!-- === "Tab 1" -->
        <!-- Markdown **content**. -->
    <!-- === "Tab 2"
        More Markdown **content**. -->
    本章节是计算机网络的第八章。有些内容可能不重要，后续应该会标注。

## 1.概述

* 网络安全基本属性：
    1. 保密性
    2. 可控性
    3. 不可依赖性
    4. 可认证性
    5. 可用性
    6. 完整性

## 2.网络攻击与威胁

* 嗅探

* 攻击I–中间人攻击：重定向受害者的流量传送给攻击者      
    ![](./img/137.png)

* 拒绝服务攻击（DenyofService）：大量的，超过处理能力的请求/流量
    * 类型：
        1. 系统缺陷：不对称协议、返回服务器状态或计算结果
        2. 洪泛攻击：控制大量僵尸机器/网络发送请求

* SYN Floods：伪造大量SYN包发送到服务器端，产生大量半连接

* 放大攻击：
    * 原理：请求所需的包很少（1个或少量），服务器端响应需要很多包，可用于放大Dos攻击效果
    * 攻击方式：伪造报文的源地址，将源地址设为攻击目标的IP地址，服务器将大量响应报文发送给目标

* MAC Flooding：
    * 攻击者对交换机发送很多非法的、包含不同源MAC地址的封包
    * 交换机会把所有这些MAC地址记录到自己的CAM(Content Addressable Memory)表之中。
    * 当这些记录超过交换机所能承载的内存的时候，MAC flooding的效果就达成了。
    * 这时，交换机就变成了集线器，对所有信息进行无定向广播。

* 安全机制与手段：
    *  Kerckhoffs 原则：即使密码系统的任何细节已为人悉知，只要密钥（key）未泄漏，它也应是安全的。秘密 = key and  秘密 ≠ 加密算法



* 认证协议  
    reflection（反射）和replay（重放）攻击不同


## 3.电子邮件安全

TLS/SSL只要求最基础的版本