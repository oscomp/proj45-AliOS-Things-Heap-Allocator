# proj45-AliOS-Things-Heap-Allocator
### 项目名称
基于AliOS Things操作系统提供用于管理堆（heap）的动态内存管理算法

### 项目描述

AliOS Things中提供k_mm_alloc与k_mm_free两个用于申请/释放动态内存的API，在某些应用场景下他们面临与C库malloc与free类似的问题：

* 无法实现使用者对内存块的字节对齐要求
* 无法实现多内存分区的分离管理（如某些芯片内存资源分为快速SRAM与普通DRAM，使用者期望能指定从SRAM或是DRAM进行申请）

本实验的目标是在AliOS Things上面实现新的堆内存管理算法，满足以上两点外部功能要求。

### 所属赛道

2021全国大学生操作系统比赛的“OS功能设计”赛道



### 参赛要求

- 以小组为单位参赛，最多三人一个小组，且小组成员是来自同一所高校的本科生（2021年春季学期或之后本科毕业的大一~大四的学生）
- 如学生参加了多个项目，参赛学生选择一个自己参加的项目参与评奖
- 请遵循“2021全国大学生操作系统比赛”的章程和技术方案要求



### 项目导师

黄震

* github

* email wuqi.hz@alibaba-inc.com



### 难度

中等



### 特征

* 使用C语言编写

* 数据结构： pool控制块结构体（每个分区对应一个），`k_pool_head`

* 需要实现以下接口：

  ```c
  // 创建内存分区，pool为输出参数
  int krhino_pool_init(k_pool_head** pool, void* pool_addr, size_t pool_size);
  // 从pool指定的内存分区，申请size大小的内存块
  void* krhino_pool_alloc(k_pool_head* pool, size_t size);
  // 从pool指定的内存分区，申请size大小且满足alignment对齐要求的内存块
  void* krhino_pool_alloc_align(k_pool_head* pool, size_t size, size_t alignment);
  // 向pool指定的内存分区，释放ptr指定的内存块
  void krhino_pool_free(k_pool_head* pool, void* ptr);
  ```

  

### 文档

硬件平台：阿里云IoT官方推出的HaaS100
软件平台：AliOS Things物联网操作系统

HaaS100快速开始文档：https://help.aliyun.com/document_detail/184184.html
AliOS Things物联网操作系统说明文档：https://github.com/alibaba/AliOS-Things/tree/dev_3.1.0_haas

### License

* [Apache-2.0](https://opensource.org/licenses/Apache-2.0)



## 预期目标

### 注意：下面的内容是建议内容，不要求必须全部完成。选择本项目的同学也可与导师联系，提出自己的新想法，如导师认可，可加入预期目标

### 第一题：多线程并发保护
确保多线程条件下，并发的申请与释放操作都能够正确执行

### 第二题：内存碎片合并
设计算法对内存碎片进行合并以减少系统内存碎片，提高内存使用效率

### 第三题

实现时可适当考虑算法性能、可维护性
设计合适的算法性能指标，并对性能进行优化后，测量内存分配算法的性能指标数据；
算法需要能够识别出内存访问越界、内存double free等问题；
考虑在系统出现问题时（如内存申请失败），打印堆内存的统计信息，协助开发者分析问题；