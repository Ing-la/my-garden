---
{"dg-publish":true,"permalink":"/operating-systems-three-easy-pieces/moc/","noteIcon":""}
---

![os cover.png](/img/user/Operating%20Systems-%20Three%20Easy%20Pieces/picture/os%20cover.png)
# 📍 MOC — 《操作系统导论：三维度理解计算机系统》

> **本知识库致敬**
>
> Remzi H. Arpaci-Dusseau & Andrea C. Arpaci-Dusseau 所著
> **《Operating Systems: Three Easy Pieces》**（中译本《操作系统导论》）
>
> 一本让操作系统不再晦涩的经典——它用"三个简单部分"拆解了计算机系统最核心的工程智慧。

---

## 缘起

操作系统是计算机系统中唯一一个"什么都要管"的软件。它管理 CPU、管理内存、管理磁盘、管理网络，还要让成百上千的程序互不干扰地协同运行。面对如此庞杂的体系，大多数人要么迷失在细节的汪洋中，要么停留在"学完就忘"的表层。

本书的伟大之处在于：它用三个纬度——**虚拟化 (Virtualization)**、**并发 (Concurrency)**、**持久性 (Persistence)**——将操作系统的核心矛盾与解决之道梳理得一清二楚。这不是一本罗列 API 的参考手册，而是一本让你真正"理解"操作系统的书。

本笔记体系遵循原书的逻辑脉络，每章提炼为六个维度：**核心矛盾 → 核心概念 → 逻辑演进 → 机制与策略 → 设计折衷 → 关键洞察**，辅以个人消化理解与硬件补充知识，形成完整的操作系统认知地图。

---

## 书的核心思想：三个简单部分

费曼有《Six Easy Pieces》讲物理学基础，本书取其意，用"三个简单部分"讲操作系统基础：

| 部分 | 核心问题 | 核心答案 |
|:---|:---|:---|
| **虚拟化** | 物理资源有限，如何让每个程序"以为"自己在独占整台机器？ | 时分共享 + 地址转换 + 分层抽象 |
| **并发** | 多个执行流共享内存，如何保证计算结果正确且可预测？ | 原子操作 + 互斥 + 同步原语 |
| **持久性** | 数据如何跨越断电与崩溃，安全地保存在磁盘上？ | 文件系统 + 日志/日志结构 + 冗余 |

**这三个部分不是独立的知识点，而是一个递进的逻辑链条：** 先通过虚拟化创造出"无限资源"的假象，再解决多执行流并发访问这些资源时的混乱，最后确保有价值的数据能在这一切动态过程中被安全地留存下来。

---

## 知识导航

### 第一部分：虚拟化（第 1–24 章）

操作系统如何为程序创造"独占整台机器"的幻觉。

| 章节       | 主题                     | 笔记文件                                               |
| :------- | :--------------------- | :------------------------------------------------- |
| **Ch1**  | 关于本书的对话                | [[Operating Systems- Three Easy Pieces/第1章对话：关于本书 (Dialogue on the Book)\|Dialogue]] |
| **Ch2**  | 操作系统介绍                 | [[Operating Systems- Three Easy Pieces/第2章：操作系统介绍 - 深度知识架构\|深度知识架构]]                 |
| **Ch3**  | 关于虚拟化的对话               | [[Operating Systems- Three Easy Pieces/第3章对话： 关于虚拟化\|Dialogue]]                      |
| →        | **抽象：进程**              |                                                    |
| **Ch4**  | 进程的抽象：进程状态、PCB、上下文切换   | [[Operating Systems- Three Easy Pieces/第4章：抽象：进程 - 深度知识架构\|深度知识架构]]                  |
| **Ch5**  | 进程 API：fork、exec、wait  | [[Operating Systems- Three Easy Pieces/第5章：插叙：进程 API - 深度知识架构\|深度知识架构]]             |
| **Ch6**  | 受限直接执行：用户态/内核态、系统调用    | [[Operating Systems- Three Easy Pieces/第6章：机制：受限直接执行 - 深度知识架构\|深度知识架构]]              |
| →        | **CPU 调度**             |                                                    |
| **Ch7**  | 调度介绍：FIFO、SJF、RR、STCF  | [[Operating Systems- Three Easy Pieces/第7章：进程调度：介绍 - 深度知识架构\|深度知识架构]]                |
| **Ch8**  | 多级反馈队列 (MLFQ)          | [[Operating Systems- Three Easy Pieces/第8章：调度：多级反馈队列 - 深度知识架构\|深度知识架构]]              |
| **Ch9**  | 比例份额调度：彩票调度、步长调度       | [[Operating Systems- Three Easy Pieces/第9章：调度：比例份额 - 深度知识架构\|深度知识架构]]                |
| **Ch10** | 多处理器调度（高级）：缓存亲和性、多核同步  | [[Operating Systems- Three Easy Pieces/第10章：多处理器调度（高级） - 深度知识架构\|深度知识架构]]            |
| **Ch11** | 关于 CPU 虚拟化的总结对话        | [[Operating Systems- Three Easy Pieces/第11章对话：关于 CPU 虚拟化的总结\|Dialogue]]              |
| **Ch12** | 关于内存虚拟化的对话             | [[Operating Systems- Three Easy Pieces/第12章对话：关于内存虚拟化\|Dialogue]]                    |
| →        | **内存虚拟化**              |                                                    |
| **Ch13** | 地址空间抽象                 | [[Operating Systems- Three Easy Pieces/第13章：抽象：地址空间 - 深度知识架构\|深度知识架构]]               |
| **Ch14** | 内存操作 API：malloc、free   | [[Operating Systems- Three Easy Pieces/第14章：插叙：内存操作 API - 深度知识架构\|深度知识架构]]           |
| **Ch15** | 地址转换机制                 | [[Operating Systems- Three Easy Pieces/第15章：机制：地址转换 - 深度知识架构\|深度知识架构]]               |
| **Ch16** | 分段 (Segmentation)      | [[Operating Systems- Three Easy Pieces/第16章：分段 - 深度知识架构\|深度知识架构]]                    |
| **Ch17** | 空闲空间管理                 | [[Operating Systems- Three Easy Pieces/第17章：空闲空间管理 - 深度知识架构\|深度知识架构]]               |
| **Ch18** | 分页：介绍                  | [[Operating Systems- Three Easy Pieces/第18章：分页：介绍 - 深度知识架构\|深度知识架构]]                 |
| **Ch19** | TLB：快速地址转换             | [[Operating Systems- Three Easy Pieces/第19章：分页：快速地址转换（TLB） - 深度知识架构\|深度知识架构]]        |
| **Ch20** | 多级页表、倒排页表              | [[Operating Systems- Three Easy Pieces/第20章：分页：较小的表 - 深度知识架构\|深度知识架构]]               |
| **Ch21** | 超越物理内存：交换空间、页面错误       | [[Operating Systems- Three Easy Pieces/第21章：超越物理内存：机制 - 深度知识架构\|深度知识架构]]             |
| **Ch22** | 页面置换策略：FIFO、LRU、近似 LRU | [[Operating Systems- Three Easy Pieces/第22章：超越物理内存：策略 - 深度知识架构\|深度知识架构]]             |
| **Ch23** | VAX/VMS 虚拟内存系统（实战案例）   | [[Operating Systems- Three Easy Pieces/第23章：VAX、VMS 虚拟内存系统 - 深度知识架构\|深度知识架构]]        |
| **Ch24** | 内存虚拟化总结对话              | [[Operating Systems- Three Easy Pieces/第24章对话：内存虚拟化总结\|Dialogue]]                    |

### 第二部分：并发（第 25–34 章）

当多个执行流共享同一地址空间时，如何保证正确性。

| 章节 | 主题 | 笔记文件 |
|:---|:---|:---|
| **Ch25** | 关于并发的对话 | [[Operating Systems- Three Easy Pieces/第25章对话：关于并发\|Dialogue]] |
| **Ch26** | 并发介绍：线程、竞态条件、原子性 | [[Operating Systems- Three Easy Pieces/第26章：并发：介绍 - 深度知识架构\|深度知识架构]] |
| **Ch27** | 线程 API：pthread_create、pthread_join | [[Operating Systems- Three Easy Pieces/第27章：插叙：线程 API - 深度知识架构\|深度知识架构]] |
| **Ch28** | 锁：TAS、CAS、自旋锁、互斥锁 | [[Operating Systems- Three Easy Pieces/第28章：锁 - 深度知识架构\|深度知识架构]] |
| **Ch29** | 基于锁的并发数据结构：计数器、链表、队列 | [[Operating Systems- Three Easy Pieces/第29章：基于锁的并发数据结构 - 深度知识架构\|深度知识架构]] |
| **Ch30** | 条件变量：生产者消费者、信号量实现 | [[Operating Systems- Three Easy Pieces/第30章：条件变量 - 深度知识架构\|深度知识架构]] |
| **Ch31** | 信号量：从二元信号量到哲学家就餐 | [[Operating Systems- Three Easy Pieces/第31章：信号量 - 深度知识架构\|深度知识架构]] |
| **Ch32** | 常见并发问题：死锁、非死锁 Bug | [[Operating Systems- Three Easy Pieces/第32章：常见并发问题 - 深度知识架构\|深度知识架构]] |
| **Ch33** | 事件驱动并发：select、epoll、异步 I/O | [[Operating Systems- Three Easy Pieces/第33章：基于事件的并发（进阶） - 深度知识架构\|深度知识架构]] |
| **Ch34** | 并发的总结对话 | [[Operating Systems- Three Easy Pieces/第34章对话：并发的总结\|Dialogue]] |

### 第三部分：持久性（第 35–45 章）

数据如何在断电与崩溃中存活下来。

| 章节       | 主题                          | 笔记文件                                          |
| :------- | :-------------------------- | :-------------------------------------------- |
| **Ch35** | 关于持久性的对话                    | [[Operating Systems- Three Easy Pieces/第35章对话：关于持久性\|Dialogue]]                 |
| **Ch36** | I/O 设备：设备驱动、MMIO、DMA        | [[Operating Systems- Three Easy Pieces/第36章：IO 设备 - 深度知识架构\|深度知识架构]]            |
| **Ch37** | 磁盘驱动器：寻道、旋转延迟、IOPS          | [[Operating Systems- Three Easy Pieces/第37章：磁盘驱动器 - 深度知识架构\|深度知识架构]]            |
| **Ch38** | RAID：0、1、4、5、10 各级别         | [[Operating Systems- Three Easy Pieces/第38章：廉价冗余磁盘阵列 (RAID) - 深度知识架构\|深度知识架构]]  |
| **Ch39** | 文件和目录：inode、目录结构、硬/软链接      | [[Operating Systems- Three Easy Pieces/第39章：插叙：文件和目录 - 深度知识架构\|深度知识架构]]         |
| **Ch40** | 文件系统实现：VSFS 详解              | [[Operating Systems- Three Easy Pieces/第40章：文件系统实现 - 深度知识架构\|深度知识架构]]           |
| **Ch41** | FFS：局部性优化、磁盘感知的文件系统         | [[Operating Systems- Three Easy Pieces/第41章：局部性和快速文件系统 (FFS) - 深度知识架构\|深度知识架构]] |
| **Ch42** | 崩溃一致性：FSCK 与日志 (Journaling) | [[Operating Systems- Three Easy Pieces/第42章：崩溃一致性：FSCK 和日志 - 深度知识架构\|深度知识架构]]   |
| **Ch43** | LFS：日志结构文件系统                | [[Operating Systems- Three Easy Pieces/第43章：日志结构文件系统 (LFS) - 深度知识架构\|深度知识架构]]   |
| **Ch44** | 数据完整性：校验和、错误恢复              | [[Operating Systems- Three Easy Pieces/第44章：数据完整性和保护 - 深度知识架构\|深度知识架构]]         |
| **Ch45** | 持久性的总结对话                    | [[Operating Systems- Three Easy Pieces/第45章对话：关于持久的总结对话\|Dialogue]]             |

### 第四部分：分布式（第 46–50 章）

当系统跨越多个节点时，如何保持一致性。

| 章节 | 主题 | 笔记文件 |
|:---|:---|:---|
| **Ch46** | 关于分布式的对话 | [[Operating Systems- Three Easy Pieces/第46章对话：关于分布式的对话\|Dialogue]] |
| **Ch47** | 分布式系统基础：通信、容错、共识 | [[Operating Systems- Three Easy Pieces/第47章：分布式系统 - 深度知识架构\|深度知识架构]] |
| **Ch48** | NFS：无状态协议、幂等操作 | [[Operating Systems- Three Easy Pieces/第48章：Sun 的网络文件系统 (NFS) - 深度知识架构\|深度知识架构]] |
| **Ch49** | AFS：回调机制、会话语义 | [[Operating Systems- Three Easy Pieces/第49章：Andrew 文件系统 (AFS) - 深度知识架构\|深度知识架构]] |
| **Ch50** | 分布式的总结对话 | [[Operating Systems- Three Easy Pieces/第50章对话：关于分布式的总结\|Dialogue]] |

---

## 专题笔记

除按章节整理的深度知识架构外，本库还包含以下专题笔记——它们或是跨章节的认知整合，或是必要的硬件背景补充：

| 笔记 | 定位 | 关联章节 |
|:---|:---|:---|
| [[Operating Systems- Three Easy Pieces/第19章补充知识：CPU 组成原理\|CPU 组成原理 —— 操作系统学习补充知识]] | 硬件视角的必修课：从 CPU 芯片内部架构（ALU、CU、寄存器、MMU、TLB、多级缓存、多核一致性）理解 OS 机制在硬件上的落地点。 | Ch4–6, Ch15–22 (进程切换、内存管理) |
| [[Operating Systems- Three Easy Pieces/第27章补充理解：个人阶段性理解-进程线程\|进程与线程 —— 个人阶段性理解]] | 从"两个最小单位"的视角出发，辨析进程树与线程的关系，拆解 fork 的多线程陷阱、堆栈对向生长的设计逻辑、线程切换为何快于进程切换。 | Ch4, Ch26 (进程、线程) |
| [[Operating Systems- Three Easy Pieces/第28章补充理解：临界区(Critical Section)和锁\|临界区与锁 —— 补充理解]] | 理清从"临界区天然存在"到"锁是后加的保护手段"的因果逻辑，辨析锁的物理本质（内存变量 + 协议）、硬件原子指令、自旋 vs 互斥、死锁场景。 | Ch28–29 (锁、同步) |
| [[Operating Systems- Three Easy Pieces/第5章补充理解：进程 API 与 Shell 工作机制\|进程 API 与 Shell 工作机制]] | 拆解 fork+exec+wait 的设计哲学与运作细节：影分身、夺舍变身、僵尸/孤儿进程、管道实现原理。 | Ch5 (进程 API) |
| [[Operating Systems- Three Easy Pieces/第17章补充理解：外部碎片(External Fragmentation)与定长分配思维\|外部碎片与定长分配思维]] | 从变长分配的困境到定长分配的哲学转换，理解外部碎片 vs 内部碎片的本质权衡，以及分页思维如何彻底消灭外部碎片。 | Ch17–18 (内存管理) |

---

## 视觉辅助（picture 目录）

本库中的自绘示意图，与笔记内容一一对应：

| 图示 | 关联章节 |
|:---|:---|
| 进程的五种状态及转换 | Ch4 |
| 受限直接执行 (LDE) 协议图 | Ch6 |
| 多级反馈队列 (MLFQ) 调度逻辑架构 | Ch8 |
| 缓存亲和性与多核调度图 | Ch10 |
| 地址空间的逻辑抽象图 | Ch13 |
| 分页地址转换逻辑图 | Ch18 |
| 多级页表架构图 | Ch20 |
| 多线程进程的虚拟地址空间布局 | Ch26 |
| 竞态条件 $1+1=1$ 的并发悲剧 | Ch26 |
| 生产者消费者——基于条件变量的同步机制 | Ch30 |
| 死锁的图论证明——资源依赖环 | Ch32 |
| 计算机系统总线层次结构图 | Ch36 |
| 磁盘物理结构与延迟图 | Ch37 |

---

## 使用建议

1. **从头到尾顺序阅读**：本书的逻辑本身就是递进的——不理解地址转换，就看不懂分页；不理解分页，就看不懂 TLB；不理解并发问题，就不知道为什么需要锁。建议至少在第一遍时按原书顺序学习。

2. **按需查阅特定主题**：每篇笔记都采用统一的"六维度结构"（核心矛盾→核心概念→逻辑演进→机制与策略→设计折衷→关键洞察），方便你在需要时快速定位某个概念在整条逻辑链中的位置。

3. **重视"对话"章节**：这些看似轻松的师生对话，实际上是每部分的"思维热身"和"认知收束"。教授和学生的问答往往直击学习者最困惑的地方——不要跳过它们。

4. **配合原书阅读**：笔记是对原书的提炼与重构，不是替代。建议先读原书对应章节，再用笔记巩固和结构化理解。书中的 "Homework" 和 "Projects" 是真正将知识内化的必经之路。

5. **善用补充笔记**：CPU 组成原理笔记为内存管理和进程调度提供了必需的硬件基础，进程线程笔记和临界区笔记则是跨章节的概念整合。它们最适合在你学完相关章节、产生疑问时作为对照阅读。

---

> *"不闻不若闻之，闻之不若见之，见之不若知之，知之不若行之。"*
> — 荀子，《劝学》
>
> 这本笔记陪伴我们走过了从"闻"到"知"的路程，而真正的"行"在对每一行代码的理解中。
>
> **致敬 Remzi H. Arpaci-Dusseau & Andrea C. Arpaci-Dusseau，**
> **感谢他们用"三个简单部分"让复杂世界变得清晰。**
