---
title: Operator System Notes
date: 2023-12-21 14:00:00 +0800
categories: [Notes]
tags: [Operator System]
pin: true
---

## 什么是操作系统？
操作系统是管理计算机硬件和软件资源，给应用程序和用户提供底层抽象的系统软件。主要功能如下：
1. **硬件管理**：管理处理器、内存、磁盘等硬件资源，高效地分配和使用计算机的计算能力。
2. **文件管理**：提供一个文件系统，用于组织、存储、管理用户的数据文件。允许用户创建、删除、读取、修改文件，提供文件保护、权限管理等功能。
3. **进程管理**：管理运行在计算机上的应用程序，负责进程的创建、调度、终止以及进程间通信等。
4. **内存管理**：管理计算机主存储器，包括内存分配、回收、虚拟内存管理等。
5. **系统安全和保护**：提供安全机制，如用户身份验证、权限管理，防止非法访问等。
6. **用户接口**：提供用户和计算机系统间的交互界面。
7. **系统服务和应用程序支持**：包括设备驱动程序、系统工具、应用程序接口等，使应用程序能够更轻松访问计算机的硬件资源和系统功能。


## 冯.诺依曼结构
冯诺伊曼结构又称为存储程序计算机，是一种计算机组织结构。主要特点是将计算机程序和数据存储同一个存储器中，通过CPU进行处理和执行。结构主要包括：
1. **存储器**：负责存储计算机程序和数据，两者共享存储空间。
2. **CPU**:负责执行存储器中的程序指令，包括：
   - 算术逻辑单元：负责执行算数和逻辑运算。
   - 控制单元：负责获取指令、解析指令，控制计算机各个部件按照指令进行操作。
3. **输入设备**：负责输入外部数据，如键盘、鼠标。
4. **输出设备**：将计算机系统处理结果输出，如显示器、打印机等。
5. **总线**：连接存储器、CPU、输入输出设备的通信通道，包括数据总线、地址总线和控制总线。

## 外部中断和异常
外中断和异常是计算机系统中用于处理非正常或特殊情况的两种机制。它们都会导致计算机暂停当前正在执行的任务，转向执行一个特定的处理程序（中断处理程序或异常处理程序），处理完特殊情况以后计算机返回到断点继续执行任务。

1. **外部中断**：由计算机外部事件触发，通常与硬件设备有关，目的是通知CPU某个事件，例如设备需要传输数据、设备发生错误等。常见外部中断源包括：
   - 输入输出设备：键盘、鼠标、磁盘在数据传输完成，缓冲区已满或发生错误时发出的中断。
    - 计时器：计时器产生的定时中断，用于时间片轮转等调度策略。
    - 电源管理：处理器进入低功耗模式发出的中断。
2. **异常**：由计算机内部事件触发，通常与正在执行的程序或指令有关，目的是通知CPU某个指令无法正常执行，需要特殊处理。常见的异常包括：
    - 算术异常：溢出、除零。
    - 地址异常：非法访问内存、页面错误。
    - 系统调用：请求操作系统提供服务时触发的异常。
    - 保护异常：试图访问受保护的资源或执行非法操作触发的异常。  
    异常发生时，处理器会跳转到异常处理程序，根据异常类型和严重程度，异常处理程序可能会修复错误、终止程序或通知用户。

## CPU地址翻译
CPU地址翻译是计算机系统将虚拟地址转换为物理地址的过程，目的是为了实现虚拟内存，让每个进程都有一致、连续的地址空间，从而简化编程和内存管理。虚拟内存到物理内存的映射方式包括分页和分段。

**分页系统**：将虚拟内存和物理内存分成固定大小的页，例如4kB。分页系统的任务就是将虚拟页映射到物理页框。

**地址翻译过程**：虚拟地址由虚拟页号和页内偏移量组成。页号标志一个页，偏移量表示在页内的位置。地址翻译包括以下步骤：
1. 虚拟地址中提取页号和页内偏移量。
2. 使用页表将页号转换为物理页框号。
3. 将物理页框号和页内偏移量组合成物理地址。

**页表查找和地址翻译**：单级页表系统中，页表是一个线性数组，使用虚拟页号作为索引查找物理页框号，缺点是对于较大的地址空间，单级页表可能非常庞大且浪费内存。多级页表系统中，页表划分为多个层次结构，虚拟内存地址被分为多个部分，每个部分用于在不同级别的页表中查找，最后一级页包含实际的物理页框号。多级页表可以有效减少内存消耗，因为只需要分配实际使用的页表空间。

**页表缓存**：为了加速地址翻译，引入了叫做Translation Lookaside Buffer(TLB)的硬件缓存。TLB缓存最近使用的虚拟页号到物理页框号的映射关系，当处理器需要地址翻译时，首先在TLB中查找，若找到了对应的映射，就不需要再访问页表，减少了访存次数和地址翻译的延迟，称为TLB命中。

**TLB特点**：
1. 容量小（数十到数百条映射），较低的访问延迟。
2. 关联性：可以是全相连、组相连、直接映射的。
   - 全相联：任何虚拟内存地址可以放在TLB任意位置。
   - 直接映射：每个虚拟内存地址固定地映射到一个位置。
   - 组相联：TLB划分为多个组，虚拟地址按照一定策略映射到特定组当中。
3. 替换策略：最近最少使用（LRU）,随机替换（Random）策略。

**工作流程**：
1. TLB命中：直接使用TLB中的物理页。
2. TLB未命中：访问内存中的页表进行地址翻译，完成后将新的映射关系添加到TLB中，以便后续访问。


## 现代CPU指令周期和指令类型
**指令周期**：CPU执行一条指令所需要的时间。在CPU中，每条指令会经过一系列阶段完成执行，这些阶段构成指令执行的流水线。经典的流水线包括以下阶段：
1. 取指：内存中获取指令
2. 解码：指令转换为控制信号和操作数
3. 执行：根据指令类型，执行相应操作
4. 访存：若指令涉及内存操作，则访问内存
5. 写回：将执行结果写回目标寄存器

**指令类型**：
1. 算术指令：加减乘除
2. 逻辑指令：与或非、异或
3. 移位指令：左移、右移
4. 控制流指令：跳转、分支、函数调用
5. 数据传输指令：加载、存储
6. 特殊指令：系统调用、同步原语、浮点运算等。


## 局部性原理
计算机在程序执行过程中，一段时间内，对内存地址的访问倾向于集中在较小的地址范围内。主要分为：
1. 时间局部性：如果某个数据或指令被访问了一次，很有可能在不久的将来再次被访问。例如循环结构中，循环体内的指令和数据会被反复访问。
2. 空间局部性：如果程序访问一个内存地址，很有可能在不久的将来访问相邻的内存地址。例如程序中的数组操作，数组元素在内存中连续存储，通常会顺序访问相邻元素。

局部性原理的应用：
- 高速缓存：Cache是介于CPU和内存之间的高速缓存器。缓存最近被访问的内存页。
- TLB：缓存虚拟页地址和物理页框之间的映射关系。
- 预取策略（Prefetching）:预测程序未来可能访问的内存地址，预先加载到cache中。
- 磁盘调度：文件系统在磁盘上存储数据时，通常将相关的数据块放在相邻的磁盘扇区上，以便在访问一个数据块时快速地访问相邻地数据块。

## CPU缓存
Cache是位于CPU和内存之间的高速存储器，用于存储近期访问过的数据和指令。目的是利用局部性原理，减少CPU访存次数，提高处理器性能。类型包括：
- L1缓存：分为L1数据缓存和L1指令缓存。容量相对较小（几十KB）,访问速度最快。
- L2缓存：存储更多数据和指令（几百KB到几MB），提高缓存命中率。
- L3缓存：容量更大(几到几十MB)，速度更慢。通常在多核处理器中共享，用于在不同核之间共享数据和降低访存的延迟。

组织方式：
1. 直接映射缓存：每个内存块号映射到缓存固定区域。简单，但容易缓存冲突。
2. 全相联映射缓存：每个主存块可以映射到缓存任意位置。降低了缓存冲突可能性，实现较为复杂，搜索缓存较慢。
3. 组相联映射缓存：折衷前两者。缓存划分为多个组，每个内存块可以映射到特定组的任何位置。

缓存替换策略：
1. 随机替换
2. 最近最少使用（LRU）
3. 最不经常使用
4. 先进先出

## CPU缓存一致性
CPU缓存一致性是多核处理器系统的一种关键技术，确保各个核之间数据一致性。
- 写操作的一致性：当一个处理器对内存某个地址进行写操作时，需要确保其他处理器对该地址的访问能够看到这次写操作的结果；如果多个处理器同时对一个地址进行写操作，需要确保它们的操作有明确顺序。
- 事务性：多核处理器的缓存一致性需要满足事务性，即对内存的操作要么完全执行，要么完全不执行；确保多个处理器之间的数据传输不会产生错误或不一致状态。
- 缓存一致性协议：
    1. MESI协议：Modified（修改）、Exclusive(独占)、Shared(共享)、Invalid(无效)四种状态。MESI协议通过对缓存行设置这四种状态来维护一致性。
    2. MSI协议：简化版MESI协议，性能相对较低。
- 缓存一致性实现：需要在硬件层面支持，例如多核处理器系统中通常包含一个或多个总线嗅探器，用于处理处理器之间的通信，以及一个或多个总线控制器，用于控制处理器之间的传输。

## NESI缓存一致性协议
通过四种状态跟踪缓存行状态：
- Modified（修改）：缓存行中数据被修改，与内存不一致。该处理器核心负责将数据写回内存
- Exclusive（独占）：缓存行中数据与内存一致，且只在当前缓存行存在。对于独占的缓存行，可以自由地写入，不需要通知其他核心。
- Shared（共享）：缓存行中数据与内存数据一致，但可能其他核心的缓存中也存在。此时多个处理器核心都可以读取该数据。
- Invalid（无效）：缓存行中数据是无效的，可能是因为其他处理器核心修改了数据，或当前处理器核心失去对该数据的独占权限。

MESI协议通过监控CPU核心的读写操作和跟踪其他CPU核心的操作来实现缓存一致性。当一个处理器核心需要执行读写操作时，发送请求到其他狠心，根据其他核心的缓存行状态来更新自己的缓存行状态。

## 伪共享问题
伪共享是多处理器系统中的性能问题，当多个核心频繁访问位于同一个缓存行内的不同数据时，可能导致性能下降.

例如变量A和B在同一个缓存行中,线程1和线程2分别访问变量A,B:
1. 线程1读A,缓存行状态为(S,S);
2. 线程1写A,缓存行状态为(M,I);
3. 线程2读B,首先将该缓存行更新到内存,状态为(S,S);

相比于变量A,B不在同一缓存行,多了大量的写入内存操作.

伪共享导致了以下问题:
1. 频繁的缓存同步操作,性能下降
2. 增加总线的流量

避免方法:
1. 数据对齐和填充,使得不同核心访问的数据位于不同缓存行
2. 优化数据布局,使同一缓存行数据由同一个处理器核心频繁访问
3. 降低共享数据的使用
4. 使用无锁数据结构

验证代码:
```cpp
#include <iostream>
#include <chrono>
#include <thread>

using namespace std;

class Pointer {
public:
    long a;
    long p1, p2, p3, p4, p5, p6, p7; // 使a,b不在同一 cache line
    long b;
};

void test1(Pointer& p) {
    for(int i = 0; i < 100000000; i++) {
        p.a++;
    }
}

void test2(Pointer& p) {
    for(int i = 0; i < 100000000; i++) {
        p.b++;
    }
}

int main() {
    auto time_begin = chrono::high_resolution_clock::now();
    auto p = Pointer();
    
    thread t1(test1, ref(p));
    thread t2(test2, ref(p));   // 线程函数的参数按值传递,引用参数必须被包装

    cout << p.a << " " << p.b << endl;
    t1.join();
    cout << p.a << " " << p.b << endl;
    t2.join();
    cout << p.a << " " << p.b << endl;

    cout << chrono::duration_cast<chrono::microseconds>(chrono::high_resolution_clock::now() - time_begin).count() << endl;
}

// Linux编译命令
// g++ ./c++/main.cpp -o main -std=c++11 -lpthread
```

## 用户态和内核态，以及切换方式
用户态和内核态是操作系统为了保护系统资源和实现权限控制而设计的两种不同的CPU运行级别。用户态是程序运行的正常状态，内核态是系统执行内核代码或相应系统调用时的特权状态。

**用户态和内核态区别**
1. **权限**：内核态可以执行所有指令和访问所有内存空间；用户态收到限制，不能直接访问内核地址空间或者执行特权指令。
2. **代码**：内核态主要执行操作系统内核代码，如中断处理程序、设备驱动、文件系统等。用户态主要执行应用程序代码。
3. **资源访问**：用户态下，程序不能访问受保护的操作系统资源，如硬件设备、中断、内核数据结构等。

**触发用户态和内核态切换的场景**
1. 系统调用：应用程序请求操作系统提供的服务时，会通过系统调用进入内核态。系统调用触发一个特殊的中断，将CPU从用户态切换到内核态。在内核态下操作系统执行相应的服务程序，完成请求后，再通过中断返回指令将CPU切换回用户态。
2. 异常：程序执行过程中出现错误或异常情况（如除零、非法指令、缺页等）
3. 外部中断：外部设备（鼠标、键盘、磁盘等）产生的中断使CPU从用户态切换到内核态。操作系统处理这些中断信号，执行相应的中断处理程序，然后再将CPU切换到用户态。

**切换过程**
1. 中断源发出中断请求
2. 判断处理机是否允许中断和该中断源是否屏蔽
3. 优先级排队
4. 保护现场：由于特权级发生变化，从用户栈切换到内核栈。旧栈的SS、ESP指针压入新栈。压入程序状态信息，即EFLAGS寄存器。压入断点，即返回地址，即当前任务的CS:EIP寄存器。若有错误码，压入错误码。
5. 定位中断服务程序：通过中断向量号定位中断服务程序地址。
6. 中断处理
7. 中断返回

## 程序的基本执行过程
1. 源代码
2. 编译:对于编译型语言,编译器将源代码编译成目标代码,称为可执行文件,包括预处理,编译,汇编,连接等步骤.
3. 加载:操作系统将代码和数据加载到内存中
4. 执行:处理器按照程序指令执行程序
5. 结束

## 并发和并行的区别
**并发**：一个时间段内同时处理多个任务的能力，强调任务之间的独立性，以及它们是如何在同一个处理器或单个核心上交替执行的。并发的目的是在有限资源下实现任务的有效调度，最大限度提高系统的响应速度。

**并行**：同一时刻执行多个任务。要求系统具有多个处理器或计算核心。目的是加快任务的完成速度，通过将任务分解成更小的部分并在多个处理器或核心上同时执行，以实现更快的执行速度。

**区别**：  
1. 并发关注同一时间段内处理多个任务，并行关注同一时刻执行多个任务。
2. 并发适用于单处理器或单核心系统，通过任务调度实现多任务处理；并行依赖多处理器或多核心系统，实现任务的同时处理。
3. 并发是为了提高系统的响应速度和吞吐量；并行旨在加快任务的完成速度。

## 什么是互斥锁、自旋锁，底层是怎么实现的
互斥锁和自旋锁是两种用于同步和保护共享资源的锁机制，它们都可以防止多个线程或进程同时访问临界资源，从而避免静态条件和数据不一致等问题。

**互斥锁**：当一个线程获得互斥锁并访问临界资源时，其他试图获得该锁的线程将被阻塞，直到锁释放。互斥锁的底层实现依赖操作系统原语，例如linux系统使用pthread库的pthread_mutex_t数据结构实现互斥锁。

**自旋锁**：低级的同步原语，通常用于多处理器和多核系统中。与互斥锁不同，当一个线程试图获取自旋锁时，如果锁已经被其他线程持有，该线程将不断循环检查锁是否可用，而不是进入阻塞状态。自旋锁适用于锁持有时间较短且线程不希望在等待锁时进入阻塞状态的场景。自旋锁的底层实现依赖原子操作和CPU指令，如测试和设置（test_and_set）或者比较和交换（compare_and swap）

互斥所和自旋锁的主要区别在于等待锁时的行为：
- 线程试图获取已被占用的互斥锁时，会进入阻塞状态，让出CPU资源，等待锁被释放。
- 线程试图获取已被占用的自旋锁时，会不断检查锁是否可用，而不会让出CPU资源。

## 死锁及处理
死锁是指多线程或多进程环境中，线程或进程相互等待彼此持有的资源，导致这些线程或进程无法继续执行的情况。

**死锁条件**：
1. 互斥条件：一个资源在同一时间只能被一个进程或线程占用。
2. 占有且等待条件：一个进程或线程至少占用一个资源，仍然尝试获取其他进程或线程锁持有的资源。
3. 不可抢占条件：资源不能被强行从一个进程或线程抢占，只能由占有它的进程或线程主动释放。
4. 循环等待条件：存在一个进程或线程等待队列，其中每个进程或线程都在等待下一个进程或线程所持有的资源，形成循环。

**处理死锁**  
分为预防、避免和恢复检测三种策略：
1. 预防死锁：破坏死锁条件中的一个或多个，例如：
    - 破坏占有等待条件：要求线程或进程在请求资源之前释放所有已经持有的资源；或一次性请求所有需要的资源。
    - 破坏循环等待条件：为所有资源分配一个全局的顺序，并要求线程或进程按照这个顺序请求资源（B+树上锁策略）。
2. 避免死锁：运行时动态检查资源分配情况，确保系统不会进入不安全状态。例如银行家算法，通过模拟资源的分配过程来判断是否会产生死锁，如果产生死锁，则拒绝分配资源。
3. 检测和恢复：允许系统进入阻塞状态，然后定期检测死锁，发现死锁后采取措施解决。常见的检测方法包括：
    - 资源分配图：查看各类资源的剩余数量和进程的资源请求，将剩余资源分配给能够完全满足的进程，等待该进程运行结束后，回收该进程资源，再查看其他进程请求是否能满足。若全部进程请求可以满足，不存在死锁，否则存在死锁。
    
    恢复死锁方法包括：
     - 终止进程/线程：强制终止，释放其持有的资源。
     - 回滚进程/线程：将进程/线程回滚到之前的某个状态重新执行，需要系统支持事务和恢复功能。
     - 动态资源分配：尝试动态地分配资源，例如向系统申请更多资源。

**银行家算法**：进程P申请资源时，首先判断剩余资源能不能满足，若能满足，试探地将申请的资源分配给P,然后判断此时系统是不是处于安全状态。判断方法是根据剩余资源，判断能不能使某个进程执行完；如果能，假设该进程已经执行完，回收了它持有的资源，然后继续判断能不能让剩余进程中的某个执行完。如果能找到一条安全序列使所有进程都执行完，那么系统就是安全的，可以满足进程P的申请。即资源分配图可完全简化，表示没有发生死锁。


## 读写锁
读写锁不同于互斥锁，允许多个线程同时读操作，但在写操作时，只允许一个线程访问共享资源，适用于读多写少的场景，可以提高多线程程序的性能。读写锁特性：
1. 共享读：多个线程可以同时获得读锁。
2. 独占写：一个线程获得写锁时，其他线程无法获得读锁或写锁。
3. 优先级：根据实现方式不同，可能优先读或者优先写，可能导致饥饿问题。

读写锁的实现依赖于底层操作系统原语。

## Linux同步机制
Linux提供多种同步机制，以实现多线程或多进程对共享资源的访问。
1. 互斥锁（Mutex）
2. 读写锁（Shared_mutex）
3. 条件变量（Condition Variables）：线程需要等待某个条件满足时，可以使用条件变量进入休眠状态，直到另一个线程更改共享资源并通知条件变量。
4. 信号量（Semaphore）：用于同步的计数器，限制对共享资源的访问数量。

## 信号量实现方式
信号量是一种同步原语，用于实现多线程和多进程之间的同步和互斥。信号量本质是一个整数计数器，通常用于限制对共享资源的访问数量。信号量涉及两个关键操作：wait(P),post(V)

**实现原理**：
1. 初始化：计数器设置为允许共同访问的共享资源的最大数量。
2. Wait(P)操作：线程想要访问共享资源时执行，信号量计数器减一，若为负值，表示没有可用资源，进程/线程阻塞，直到有资源可用。
3. Post(V)操作：完成共享资源访问后，执行post操作，信号量计数器加一，若此时计数器值小于等于0，表示有等待的进程/线程，此时会唤醒一个被阻塞的进程/线程。

## 条件变量实现方式
条件变量是用于实现线程间同步的原语，允许线程等待某个条件满足，条件满足时，其他线程会通知该线程。条件变量通常与互斥锁一起使用，以保护共享资源的访问和同步。

1. 初始化
2. 等待条件：当一个线程需要等待某个条件满足时，会执行以下操作：
    - 线程获取互斥锁，保护共享资源的访问。
    - 线程检查条件是否满足，不满足，阻塞并等待通知，进入阻塞状态前，自动释放关联的互斥锁。
    - 当条件变量收到通知，线程唤醒，自动重新获取关联的互斥锁。
    - 唤醒后，重新检查条件是否满足，避免假唤醒情况。
3. 通知条件：当一个线程改变了共享资源，并使条件满足时，执行以下操作：
    - 线程获取互斥锁
    - 线程修改共享资源，使条件满足
    - 线程通知其他一个或所有线程
    - 线程释放互斥锁。


## 生产者消费者问题
一组生产者和一组消费者线程/进程，共享有限容量的缓冲区。生产者负责将数据放入缓冲区，消费者负责取出数据进行处理。问题核心在于如何实现对共享缓冲区的同步访问。

**关键点**：
1. 进程同步问题：缓冲区为空，消费者不能取出；缓冲区满，生产者不能写入。需要使用同步原语（互斥锁、信号量、条件变量）来实现。
2. 互斥：各线程互斥访问缓冲区。
3. 缓冲区管理：适当数据结构来存储缓冲区数据，如队列、栈、循环缓冲区。同时需要考虑缓冲区容量限制。

**方法1：信号量和互斥锁**
1. 初始化两个信号量：一个表示空闲缓冲区数量(E)；一个表示已使用缓冲区数量(U)。
2. 初始化一个互斥锁，用于保护对缓冲区的访问。
3. 生产者：
    1. P(E)
    2. 获取锁
    3. 生产数据
    4. 释放锁
    5. V(U) 
4. 消费者：
   1. P(U)
   2. 获取锁
   3. 消费数据
   4. 释放锁
   5. V(E)

**方法2：条件变量和互斥锁**
1. 初始化一个互斥锁，用于保护共享缓存区。
2. 初始化两个条件变量，一个表示缓冲区非空，另一个表示缓冲区空
3. 生产者：
   1. 获取互斥锁，保护共享缓冲区访问。
   2. 缓冲区已满时，等待非满的条件变量。
   3. 放入数据
   4. 通知缓冲区非空的条件变量，表示有数据可以取出。
   5. 释放互斥锁。
4. 消费者：
   1. 获取互斥锁
   2. 缓冲区为空时，等待非空的条件变量
   3. 取出数据
   4. 通知缓冲区非满条件变量，表示有空间可供放入
   5. 释放互斥锁

条件变量方案更加直接，且可能具有更好的性能。

## 哲学家进餐问题
五位哲学家围坐在圆桌上，共享五根筷子。

**解决方案**：
1. 资源分级：间隔地分为低级和高级，每个哲学家总是优先拿起高优先级的筷子（死锁预防，破坏循环等待条件）
2. 限制同时进餐人熟
3. 使用信号量或条件变量


## 内存虚拟化
内存虚拟化是将物理内存资源抽象、管理、分配的技术，允许将计算机的物理内存划分为独立、隔离的虚拟内存块。

**主要目的**
1. 资源隔离和共享：在不同进程或虚拟机之间隔离内存资源，提高系统的稳定性和安全性。同时，内存虚拟化支持灵活地共享内存资源，实现负载均衡和资源利用率最大化。
2. 易用性：简化内存管理，由操作系统和硬件负责处理内存分配、回收等问题。
3. 容错和恢复：系统故障时，可以将虚拟内存状态保存在磁盘上，然后在另一台计算机上恢复虚拟内存状态，以实现快速恢复。
4. 内存优化：支持内存优化技术，如按需分配、内存去重、内存压缩，可以提高内存资源的利用率。
5. 进程保护：每个进程都有自己的虚拟内存空间，防止一个进程额外或恶意地访问另一个内存的进程，有助于提高系统的安全性和稳定性。

## 逻辑地址和物理地址
**逻辑地址**：逻辑地址是相对于每个进程的，每个进程都有自己的的逻辑地址空间。通过内存管理单元（MMU）映射到物理地址，以访问实际的物理内存。这种映射机制使得每个进程都可以认为自己拥有连续、独立的内存空间，无需关心其他进程和物理内存的实际布局。

**物理地址**：实际内存硬件的地址，用于在物理内存中定位数据。物理地址是全局唯一的，直接表示物理内存中的位置。


## 操作系统在内存管理时做了什么
操作系统负责为进程分配、管理、回收内存资源，主要负责执行以下任务：
1. 内存分配：操作系统会维护一个内存空闲列表或内存池，用来追踪可用的内存块。当进程申请内存时，操作系统会分配合适大小的内存块。
2. 地址空间管理：操作系统通过内存管理单元将虚拟地址映射到实际的物理内存地址。
3. 内存保护：操作系统确保每个进程的内存空间不被其他进程非法访问，确保系统的稳定性和安全性。
4. 内存回收：进程终止和释放时，操作系统负责回收已经分配的内存资源。回收的内存将返回空闲内存列表或内存池。
5. 页面置换：使用页面置换算法（LRU、FIFO等）管理内存页面。当物理内存不足以容纳新页面的时候，操作系统会选择一个合适的页面将其从内存换出，为新页面腾出空间。
6. 内存优化：通过内存去重，内存压缩、按需分配等技术提高内存资源的利用率。

## 物理内存和虚拟内存的映射机制
映射方式一般分为分段和分页，分段机制由于内存碎片较多，常用分页机制。映射过程由内存管理单元和操作系统共同完成。

**分页机制**：虚拟内存和物理内存都被划分为固定大小的单元，称为页。虚拟页和物理页大小相同，通常是4KB。

**页表**：存储虚拟页面号到物理页框号的映射关系的数据结构。每个进程都有自己的页表，由操作系统管理。

**地址转换**：虚拟地址划分为虚拟页号和页内偏移，虚拟页号用来查询页表中对应的物理页框号，页内偏移量表示在物理页中的具体位置。

**页面置换和缺页中断**：虚拟页尚未加载到物理内存时，发生页面缺失。在这种情况下，操作系统需要从磁盘加载所需要的页面，映射到物理内存。为了腾出空间，操作系统可能需要选择一个已加载的页面，将其换出到磁盘。页面置换算法用于决定哪个页面需要被换出。

**多级页表**：用于减小页表大小。多级页表将虚拟内存地址空间划分成多个层次来减少表的大小，每个层次都有自己的页表，只有在需要的时候才会分配，大大减小内存开销。

**快表**：Translation Lookaside Buffer (TLB)，是一种硬件的高速缓存，用于加速地址翻译的过程。当MMU需要转换一个虚拟地址时，先检查TLB是否有响应缓存，如果有就不需要访问内存查找页表，从而加快地址翻译速度。

**内存分配策略**：
1. 按需分配：只在进程实际访问虚拟内存时才将虚拟页加载到物理内存中。
2. 预取：根据进程的访问模式提前加载可能需要的虚拟页，以减少页面缺失的开销。

**内存共享**：允许多个进程访问相同的物理内存区域的技术。通过将不同内存的虚拟地址映射到同一个物理页，操作系统可以实现内存共享，用于共享库、进程间通信、内存去重等场景。


## 什么是换页机制
当物理内存容量不够的时候，操作系统将若干物理页的内容写出到磁盘，然后回收这些物理内存页以供使用。例如进程A的虚拟页面V映射到物理页P,操作系统希望回收P的时候，需要将P的内容写出到磁盘，并且在A的页表中去除V的映射关系，然后记录该物理页写出到磁盘的地址。此时物理页就可以被操作系统回收并且再次被分配使用。程序A的虚拟页V此时处于已经分配但未映射到物理内存的状态。

## 操作系统的缺页中断
程序访问一个尚未加载到物理内存的虚拟内存地址时，CPU会触发一个缺页中断，从而加载相应的内存页面。缺页中断过程：
1. 检查虚拟地址是否有效。
2. 查找一个空闲的物理内存页框存储所需要的虚拟内存页。
3. 如果没有找到，选择一个已加载的页面进行替换，如果页框为脏，需要将其写出到磁盘以释放页框。
4. 读取所需内存页并加载到新分配的页框中。
5. 修改页表，将虚拟地址映射到物理页框。
6. 恢复程序的执行。

**优化策略**：缺页中断涉及较慢的磁盘IO,可以采用以下策略减轻缺页中断对性能的影响：
1. 缓存和缓冲：缓存暂存最近访问过的磁盘数据，提高读取性能；缓冲合并多个连续的写操作，减少磁盘写入次数。
2. 预取：预先加载可能被访问的内存页，减少缺页中断的发生。
3. 页面置换算法：LRU、LFU、CLOCK
4. 写回策略和写穿策略：写回允许操作系统在将内存页写回磁盘前缓存修改过的数据，减少磁盘写入速度；写穿则要求修改内存页时都要将更改立即写入磁盘，确保数据一致性，但会增加磁盘写入速度。

## 缺页中断的事件顺序：
1. CPU进入内核态，然后保护现场，进入缺页中断的处理程序。
2. 操作系统检查要访问的虚拟内存地址是否有效，地址有效且没有发生保护错误，检查是否有空闲页框；没有则执行页面置换算法找一个页面淘汰。
3. 如果选择的页框脏，需要写回磁盘，挂起缺页中断的进程，直到磁盘传输结束。该页框被标记为忙，避免其他进程占用。
4. 页框干净后，操作系统查找所需页面在磁盘上的地址，通过磁盘IO写入。缺页中断的进程仍然被挂起。
5. 磁盘中断发生后，表明页已经装入，页框被设置为正常状态。
6. 恢复现场，返回用户空间继续执行。

## 换页抖动
系统频繁缺页中断并进行换页操作时，导致系统性能急剧下降。抖动现象下，CPU大部分时间都用于处理缺页中断和换页操作，而不是执行实际的应用程序，导致系统吞吐量和相应时间变差。

**原因**：
1. 过高的内存的需求
2. 不下当的内存分配
3. 不合理的页面置换算法

**解决方法**：
1. 内存管理优化
2. 资源控制
3. 内存扩展
4. 调整工作负载
5. 使用交换空间


## 进程的内存分布
1. 代码区（Text Segment）:包含进程的可执行代码，通常只读，防止程序在运行时意外修改自己的代码。代码段大小在加载时确定，且在运行过程中保持不变。
2. 数据区（Data Segment）:包含进程的全局变量和静态变量，可读可写，程序加载时由操作系统分配。数据区分为a.已初始化数据区：存放已初始化的全局变量和静态变量；b.未初始化数据区：存放未初始化的全局变量和静态变量，加载时清零。
3. 堆区（Heap Segment）:存储动态分配的内存，程序运行时通过内存管理函数在堆区动态分配和释放内存。堆区内存由操作系统管理，堆区大小在运行时动态增长或缩小。
4. 栈区（Stack Segment）:存储函数调用过程的局部变量、函数参数、返回地址等信息。每个线程有自己独立的栈空间。栈区采用先进先出原则进行内存分配和释放，使栈区内存管理效率很高。栈区大小在进程运行过程中可能发生变化，但通常受到一定限制。
5. 内核空间：操作系统内核代码和数据所占用的内存区域。每个进程的内核空间通常映射到相同的物理内存区域，以便操作系统在不同进程间共享数据和代码。

## 堆上建立对象快还是栈快?
通常栈上更快：
1. 栈上分配内存在编译的时候就已经决定好了。堆上分配内存需要先找到一块空闲区域，再去分配。
2. 缓存局部性：栈上分配的内存时连续的且与程序执行顺序密切相关，栈上对象通常有更好的缓存局部性。堆上分配内存可能在物理地址上不连续，导致缓存命中率降低，影响程序执行速度。

**栈上对象的优点**：
1. 减少碎片化：栈上分配内存通常是连续的，减少内存碎片化问题。堆上内存的动态分配和释放可能导致内存空间出现不连续的空闲区域。
2. 释放对象：栈上分配对象时，对象会在离开作用域时自动释放；堆上对象需要手动释放内存，否则可能导致内存泄漏。

栈上对象的生命周期受到限制，同时，栈空间的大小通常受限（通常1M,可修改）。因此堆空间较灵活，也更大，在32位系统下，堆内存可以达到2.9G，几乎占满3G的用户空间。

**内存分配方式**：
1. 静态内存分配：程序编译期为全局变量和静态变量分配内存。这些变量在程序整个生命周期存在，不需要显式释放。通常在程序数据区。
2. 栈内存分配：函数调用过程中的局部变量、函数参数、返回地址分配内存的过程。在程序运行时进行，采用先进先出的方式进行分配和释放。栈内存分配速度快，但受到栈空间大小限制，且生命周期受到作用域限制。
3. 堆内存分配：程序运行时通过内存管理函数显示地申请和释放。可以灵活调整大小和生命周期，但分配速度较慢，可能导致内存碎片化。可以设计内存池来提高性能。


## 页置换算法
1. 最佳置换算法：选未来最长时间不会被访问的页面。理论上限，无法实现。
2. 先进先出算法
3. 最近最少使用算法（LRU）
4. 时钟置换算法（CLOCK）
5. 随机置换算法

## 64位电脑申请80G空间？32位？
64位可行，只要实际的物理内存和虚拟内存配置能够支持需求。

32位电脑地址总线宽度是32位，理论上最大内存空间4GB，因此直接在32位电脑上申请80G是不可能的。

## Malloc实现方法
malloc是C语言标准库用于动态内存分配的函数，通常使用以下几个步骤完成内存分配：
1. 初始化内存池：内存池是预先分配的一大块内存空间，用于满足内存分配需求。初始化过程包括从操作系统请求内存（如使用sbrk或mmap系统调用），建立数据结构跟踪可用的内存块。
2. 查找合适的内存块：收到分配请求时，在free list中查找一个大小满足需求的内存块，查找策略有：首次适配（first fit）,最佳适配（best fit）,最差适配（worst fit），查找策略会影响内存分配的性能和内存碎片化程度，如果找不到足够大小内存，会向操作系统申请，小于128KB使用sbrk,大于128KB使用mmap系统调用。
3. 分割内存块：如果找到的内存块远大于请求的内存大小，malloc可能将其分割成两部分，一部分满足当前需求，另一部分保存在free list中以供后续分配。
4. 更新数据结构：malloc将找到的内存块从free list中移除，更新相关的数据结构。此外，malloc通常会在返回的内存块前附加一些元数据（如内存块大小），以便后续内存释放和重新分配。
5. 返回内存块地址：返回分配的内存地址，分配的内存块内容可能是未初始化的。


## mmap实现方法
mmap是一种将文件或其他对象映射到虚拟地址空间的内存映射技术，在Unix和类Unix系统中实现为一个系统调用。mmap实现的概述如下：
1. 参数检查：应用程序调用mmap时，操作系统首先检查参数合法性，包括文件描述符、映射长度、访问权限、文件偏移等。
2. 创建虚拟内存区域（VMR）:操作系统为请求的映射创建一个虚拟内存区域，长度由调用参数确定。
3. 建立文件和虚拟内存区域的关联：操作系统将要映射的文件和新创建的虚拟内存区域建立关联，这种关联可以是“私有”或“共享”。私有映射意味着对映射区域的修改不会影响原始文件，共享映射意味着会同步到原始文件。这种映射关系通常存储在内核的页表或其他数据结构中。
4. 延迟加载：仅在应用程序实际访问映射区域时才加载所需的文件内容，从而提高性能并减少不必要的内存使用。
5. 缺页中断：应用程序访问尚未加载的映射区域时，操作系统产生一个缺页中断，查找与虚拟地址相关联的文件和偏移，将所需的文件内容加载到物理内存，并更新页表以建立虚拟地址到物理地址的映射。之后，应用程序可以继续访问映射区域。
6. 内存回写：对于共享映射，操作系统通常采用一种称为写回的策略，即一段时间后或内存压力增大时将修改后的内存写回文件。
7. 释放内存映射：当应用程序不再需要内存映射时，通过munmap系统调用释放映射区域。包括：
   1. 写回修改内容
   2. 释放相应的物理内存页
   3. 从虚拟地址空间删除映射区域，删除相关的页表条目和其他内核数据结构。

## 共享内存实现方法
共享内存是一种进程间通信机制，允许多个进程访问同一块内存区域。相同的物理内存区域被映射到每个进程的虚拟地址空间，从而实现数据共享。共享内存机制可以提高数据传输效率，避免数据复制和内核与用户空间之间的上下文切换。共享内存的实现包括：
1. 创建内存共享区域：通过shmget系统调用创建一个共享内存标识符（shared memory identifier）,用于唯一标志共享内存区域。
2. 将共享内存区域映射到进程地址空间：每个需要访问共享内存的进程将其映射到自己的虚拟地址空间。
3. 读写共享内存：通过映射到其虚拟地址空间的共享内存区域来读写数据。为了避免数据竞争和不一致，进程之间需要协调对共享内存的访问，通常使用同步原语（互斥锁、信号量等）实现。
4. 取消映射共享内存区域：不再访问共享内存时，将其从虚拟内存空间取消映射。
5. 删除共享内存区域：所有进程不再需要时，删除以释放内存资源。


## 进程、线程、协程的区别和联系
**进程（Process）**：操作系统进行资源分配和调度的基本单位，是一个独立运行的程序实体。每个进程拥有独立的内存空间、文件描述符、寄存器状态等资源。进程之间的资源是相互隔离的，因此进程间通信需要通过操作系统提供的特定机制，如管道、消息队列、共享内存等。由于进程拥有独立的资源，所以进程间切换和调度开销较大。

**线程（Thread）**：线程是操作系统调度的最小单位，是进程内的一个执行流。一个进程可以拥有多个线程，共享进程的资源，如内存空间，文件描述符等。由于线程共享相同资源，线程间通信较为简单，可以直接通过共享变量、锁等方式进行。线程相比于进程，上下文切换和调度的开销较小。但多个线程并发执行时，需要处理好同步和互斥关系，避免数据不一致或竞争条件。

**协程（Coroutine）**：用户态的轻量级线程，调度和切换完全由程序控制，不依赖操作系统的调度。协程之间共享线程的资源，因此协程间通信也可以通过共享变量、锁等方式进行。协程的优势能够轻松实现高并发，因为协程切换和调度的开销很小。协程适用于I/O密集型任务，通过异步I/O有效提高程序性能。

**联系**：
- 线程属于进程，多个线程共享进程的资源。
- 协程属于线程，多个协程共享线程的资源。
- 进程、线程、协程执行程序时，都要面对同步、互斥、通信等问题。


## 用户线程和内核线程
**用户线程**：完全在用户空间实现和管理的线程，它们的创建、同步和调度由用户级的线程库处理，而不需要内核直接参与。优点是不涉及系统调用，创建和切换的开销较小。主要限制是不能充分利用多核处理器的并行能力。当一个用户线程阻塞，如IO操作，整个进程都会阻塞，即使其他用户线程处于就绪状态。

**内核线程**：操作系统内核直接支持和管理的线程。内核负责创建、调度和销毁内核线程，每个内核线程都有独立的内核栈和线程上下文。由于内核线程是操作系统的基本调度单位，可以充分利用多处理器系统的并行能力。主要缺点是创建、调度、同步等涉及系统调用，存在较大开销。此外，内核线程需要更多内核资源，在大量线程下导致资源耗尽。

**混合模式**：用户线程和内核线程结合，每个用户线程映射到内核线程，可以同时利用用户线程的轻量级特性和内核线程的并行能力。

## 一个进程可以创建多少线程
没有固定的上限，受到以下因素制约：
1. 操作系统限制
2. 系统资源限制
3. 程序设计要求


## 进程间调度算法
**时间片轮转（RR）**
每个进程独占CPU一个时间片。时间片大小越小，任务响应越快，但调度次数增加，调度开销增大。策略弊端是任务平均周转时间较高。RR策略保证了任务公平性，但损失了性能。

**优先级调度**：在RR基础上，将时间片分配给优先级高的作业。交互式任务优先级高于批处理任务；对于有明确截止时间的任务，应设置最高优先级。明确截止时间任务>交互式任务>IO密集型任务>批处理任务。

**先到先服务（FCFS）**：任务达到时间早的优先级高，非抢占式调度。对短任务、IO密集型任务、交互式任务不友好。

**最短任务优先（SJF）**：执行时间最短的任务优先，非抢占式调度。相比于FCFS，平均周转时间短，但必须预知任务运行时间，且依赖任务到达时间。

**最短完成时间优先（STCF）**：任务剩余时间短的优先，抢占式调度。平均周转时间小，但可能导致长任务饥饿。

**多级队列（MLQ）**：静态优先级调度策略。每个不同优先级设置一个队列，优先处理优先级高的任务，同优先级之间采用时间片轮转策略保证响应时间。但存在低优先级饥饿问题。可能导致优先级反转问题：任务A,B,C优先级从高到低，C持有锁L,A等待锁L,B先运行，此时有B优先于A运行的假象。

**多级反馈队列（MLFQ）**：动态优先级调度策略，从历史经验预测作业时间。多级反馈队列包含多个优先级不同的就绪队列，每个作业只能存在于一个队列中。  
1. 总是优先执行较高优先级的作业。
2. 对于同一个队列，采用时间片轮转调度。
3. 作业进入系统时，放在最高优先级。
4. 用完整个时间片后，优先级降低：不知道作业时间时，假设高优先级的短作业，随着时间推移被认为是长作业，降低优先级。
5. 时间片内主动释放CPU,优先级不变：如果交互性工作中有大量IO操作，会在时间片用完前放弃CPU（等待用户键盘或鼠标输入）,此时保持它优先级不变，从而让交互式作业快速运行。
6. 过一段时间，将系统中所有工作重新加入最高优先级队列。首先，低优先级作业不会饿死；其次，CPU密集型作业转变为IO密集型作业时，调度程序可以正确处理
7. 为了防止CPU密集型作业在每个时间片末尾主动释放CPU来愚弄调度器，将规则4、5修改为记录每个进程在某一优先级队列中的总时间，只要进程用完配额，就会下调优先级。真正的IO密集型作业用完配额的速度较慢，而试图愚弄调度器的进程会很快用完配额，优先级被下调。

调度规则总结：
- 规则1：如果A的优先级 > B的优先级，运行A（不运行B）。
- 规则2：如果A的优先级 = B的优先级，轮转运行A和B。
- 规则3：工作进入系统时，放在最高优先级（最上层队列）。
- 规则4：一旦工作用完了其在某一层中的时间配额（无论中间主动放弃了多少次CPU），就降低其优先级（移入低一级队列）。
- 规则5：经过一段时间S，就将系统中所有工作重新加入最高优先级队列。


## 进程间通信方式
**管道**：
- 单向的进程间通信，在内核空间有一定长度的缓冲区，传输的数据是字节流。系统调用提供两个文件描述符供用户读写文件。
- 管道写端没有被进程持有，读端读取时会收到EOF，若写端被持有，读端阻塞等待。
- 匿名管道由pipe()创建，只返回两个文件描述符号，pipe[1]写给pipe[0],一般用于关系较近的进程，如父子进程。
- 命名管道创建命令为mkfifo，需要指定全局文件名和权限，读写管道就是读写这个文件。

**消息队列**
- 唯一一个以消息为数据抽象的通信方式，消息队列在内核中数据结构是一个单链表组成的队列。
- 消息队列内存空间有限，传递长消息采用共享内存方式，而不是消息队列。

**信号量**
- 用于进程间同步，控制对有限的共享资源的访问。
- 信号量操作包括P操作和V操作。P操作先将信号量减1，小于0就会阻塞；V操作将信号量加1，可以唤醒一个因P操作阻塞的进程。

**共享内存**
- 允许一个或多个进程虚拟地址映射到相同物理页，从而进行通信。


## 进程虚拟化
进程虚拟化允许多个进程并发运行，为每个进程提供独立的虚拟地址空间和资源。进程虚拟化的目标是提高资源利用率、隔离进程以保证系统安全性和稳定性，以及简化进程管理和调度。

- 虚拟地址空间：每个进程拥有独立的虚拟地址空间，包含了代码、数据、堆、栈等内存区域。虚拟地址空间通过内存管理单元映射到物理内存。虚拟内存简化了内存管理和保护。
- 上下文切换：进程虚拟化中，操作系统需要在不同进程间切换，实现多任务和并发运行。上下文切换是保存当前进程的状态（寄存器、程序计数器、内存映射等），恢复另一个进程的状态。上下文切换会导致一定的性能开销，因此需要在进程调度和同步中尽量减少不必要的切换。
- 进程隔离：确保一个进程的错误或恶意行为不会影响其他进程和系统。进程隔离通过虚拟地址空间、内存保护、权限控制等机制实现。
- 进程调度：操作系统根据优先级、资源需求和策略等因素调度进程，旨在提高资源利用率、降低响应时间和确保公平。进程调度算法包括：先来先服务、短作业优先、时间片轮转、优先级调度、多级反馈队列等。

## 进程状态
进程状态描述进程在生命周期的各种可能阶段，操作系统根据进程状态管理和调度进程。进程状态包括:
- 新建态（new）：操作系统分配必要的资源，如内存、文件描述符，并初始化控制块等数据结构。
- 就绪态（Ready）：进程已经准备好运行，等待操作系统分配时间片。就绪态的进程已经分配得到除CPU以外的所有资源，只需要CPU时间片就可以开始运行。
- 运行态（Running）：进程在CPU上执行，在任意给定时刻，每个CPU或核心只有一个进程处于运行态。
- 阻塞态（Blocking）：进程因等待某个事件（IO操作完成、锁释放、信号量等）而暂停执行，直到等待的事件发生。
- 终止态（Terminated）：进程已经执行完或被终止。进程资源被回收，进程控制块可能保留一段时间以便父进程获取子进程的退出状态。

## 进程创建过程
1. 调用fork()系统调用。系统调用复制父进程的进程控制块（PCB）,虚拟内存布局，文件描述符等数据结构，创建一个与父进程几乎完全相同的子进程。fork()在父进程中返回子进程的pid，在子进程中返回0。
2. 子进程修改内存映射：子进程采用写时复制机制，共享父进程的内存页面，直到需要修改页面时才复制页面。
3. 调用exec()系统调用：替换当前的程序映像，加载新程序的代码和数据到内存，设置程序计数器指向新程序的入口。exec()不会影响进程控制块、文件描述符等数据结构。
4. 子进程开始执行：执行新程序或继续执行父进程代码，通常进程通过fork()返回值判断的角色，执行相应的逻辑。如子进程可能会关闭不需要的文件描述符、初始化资源、启动新的线程。
5. 父进程等待子进程：父进程可以选择等待子进程完成，从而获取子进程的退出状态和回收资源。


## 如何回收线程
线程回收是指一个线程完成执行后，释放其占用的资源并清除相关数据结构的过程。

1. join()方法：主线程等待目标线程完成，并在完成后回收资源，避免内存泄漏和僵尸线程等问题。
2. detech()：线程分离，线程在退出时自动回收资源
3. 使用线程局部存储（Thread local storage, TLS）：使用TLS为每个线程分配独立的资源，如内存、文件描述符，通过TLS可以确保线程在退出时自动回收资源。


## 进程终止方式
进程终止是指一个进程完成其生命周期并释放占用的资源的过程。终止方式包括：
1. 正常终止（Normal Termination）:自然完成任务并主动退出，返回一个退出状态码，如main函数的返回值。
2. 异常终止（Abnormal Termination）:进程因为某种错误或异常而被迫退出，如段错误、浮点异常等，操作系统通常终止进程并生成一个核心转储文件（Core Dump）.
3. 通过信号（Signal）终止：操作系统使用信号机制向进程发送事件和命令，部分信号可以导致进程终止。例如，用户按下Ctrl+C时，操作系统向进程发送SIGINT信号，请求进程终止。
4. 通过系统调用终止：进程可以通过调用exit()等系统调用终止自己。这些系统调用会通知操作系统回收进程资源，并将进程状态设置为终止。
5. 父进程终止子进程：父进程可以通过特定的系统调用或信号来终止子进程，如kill()系统调用。

## 进程后台运行
1. 在Unix/Linux shell中，命令后添加 & 符号将进程放入后台运行，不会阻塞shell.
2. nohup命令使进程忽略SIGHUP信号，进程在终端关闭后不会终止。

## 守护进程、僵尸进程、孤儿进程
**守护进程**：在后台运行的特殊进程，通常用于提供某种服务或执行定期任务。守护进程没有控制终端，不会与用户交互，通常在系统启动时启动，在系统关闭时终止。守护进程名称通常以d结尾，如sshd, httpd等。创建守护进程操作：
1. fork()产生子进程，父进程退出。子进程成为孤儿进程，被init进程收养（pid=1）,摆脱原始的控制终端。
2. 调用setsid()创建新会话并成为会话组长，确保进程不再拥有控制终端。
3. 改变当前工作目录（如根目录）
4. 重设文件权限掩码
5. 关闭不需要的文件描述符
6. 处理有关信号（如 SIGHUP、SIGTERM 等）。

**僵尸进程**：已经终止但仍然占用进程表空间的进程。当一个子进程终止时，父进程没有及时回收子进程资源，子进程就成为僵尸进程，直到父进程通过调用wait()等系统调用回收资源。僵尸进程不再占用CPU或内存，但占用进程表空间。如果产生大量僵尸进程，可能导致进程表空间耗尽，影响系统性能。

**孤儿进程**：父进程在子进程之前终止，导致子进程失去父进程。在Unix/Linux系统中，孤儿进程会被init进程收养。


## 父进程、子进程、进程组、会话
**父进程、子进程**：创建新进程的进程称为父进程，新创建的进程称为子进程，形成一种进程树结构，根进程是所有进程的祖先。

**进程组**：一个或多个进程的集合，共享相同的进程组ID(Process Group ID, PGID). 进程组用于组织具有相关任务的进程，并允许向整个进程组发送信号。PGID通常由进程组的第一个进程的PID决定。进程可以通过setpgid()系统调用加入或创建一个进程组。

**会话**：一个或多个进程组的集合，共享相同的会话ID（Session ID，SID）。会话用于管理终端或登录环境下的进程，每个会话有一个单独的控制终端（Control Terminal）,控制终端可以发送信号给会话中的所有进程。


## 多进程与多线程怎么选择
**应用场景**：需要独立地址空间和资源，或需要在不同的安全上下文中运行，多进程可能更好；高度共享数据或资源，需要轻量级的并发，多线程更好。
**资源需求**：资源受限情况下，考虑资源占用较低的多线程
**开发和维护难度**：进程间通信相对复杂，需要使用管道、共享内存、信号量之类的通信机制，进程的创建和管理相对较重，可能增加开发维护难度；多线程通信较为简单，但可能涉及复杂的同步和锁机制。
**可扩展性**：多进程可以更好利用多核处理器和分布式系统；多线程在单核处理器上可能表现更好，共享资源且减少了上下文切换开销。
**容错和隔离**：多进程每个进程有独立的地址空间，有助于提高系统的容错性和隔离性。


## 进程切换情况
1. 时间片到期：进程时间片用完，被剥夺处理机，进入就绪队列等待调度。
2. 高优先级进程就绪
3. 进程自愿让出CPU：进程等待某个事件时（IO操作，锁释放）
4. 中断处理：CPU收到某个信号，当前执行的进程可能被暂停，以便系统处理中断。


## 管道通信的实现原理
管道是一种基于操作系统内核缓冲区的进程间通信机制。通过创建管道并使用文件描述符进行读写操作，进程可以在不需要额外用户空间的情况下进行通信。


## 进程切换慢，线程切换快？
1. 上下文切换范围：进程切换时，操作系统需要保存和恢复更多上下文信息，如内存管理信息等，线程切换仅需要保存和恢复线程特有上下文信息，如寄存器状态、栈指针、程序计数器等，不需要保存线程间共享的资源状态。
2. 缓存效应：线程共享进程的地址空间和资源，CPU缓存命中率可能更高。进程切换时，由于地址空间和资源的变化，CPU缓存可能需要重新填充，导致性能损失。


## 文件系统虚拟化
文件是有名字的字符序列，序列内容为文件数据，序列长度、修改时间等描述文件属性的其他信息称为文件元数据。

文件系统时管理文件数据和文件元数据的系统。文件系统提供文件抽象和访问文件需要的接口。问价以数据块为单位访问，一般为4KB.

应用程序的访问或写入一些数据到存储设备上时，采用open(),write(),read()等系统调用；处理系统调用时，内核调用虚拟文件系统（VFS）来处理文件请求。虚拟文件系统负责管理具体的文件系统，如inode文件系统，FAT32文件系统。

## 硬链接和符号链接
两种不同的文件链接类型，用于创建文件或目录引用

**硬链接**：文件系统中一个文件的额外引用。在unix文件系统中，每个文件有一个称为inode的数据结构存储文件的元数据，例如文件权限、所有者、大小。每个文件都有一个或多个文件名（硬链接），它们指向相应的inode。硬链接是文件名和inode之间的关联。

硬链接特点：
1. 不能跨文件系统。硬链接直接关联到inode，只能在文件系统中创建。
2. 不能引用目录，防止文件系统中出现循环引用和其他不一致问题。
3. 删除一个文件硬链接会导致文件被删除。最后一个硬链接删除时，文件系统将释放inode和占用的存储空间。
4. 硬链接不影响原始文件的访问，所有硬链接指向相同的inode.

**符号链接**：一种特殊的文件，包含指向另一个文件或目录的路径。符号链接通过路径来引用目标文件。当用户或程序访问符号链接时，文件系统会自动重定向到目标路径。

符号链接特点：
1. 可以跨文件系统
2. 可以引用目录
3. 删除符号链接不会影响目标文件
4. 符号链接可能引起死链接，如果目标文件被删除，符号链接指向一个不存在的路径，导致死链接。

## Inode文件系统结构
Inode（索引节点）是文件系统中的一个数据结构，用于存储文件或目录的元数据，如文件大小，时间戳、所有者、权限等，还包含指向实际文件数据块的指针，这些数据块存储了文件的内容。

在Unix/Linux文件系统中，每个文件或目录都有一个唯一的Inode号，用于唯一标识文件系统中的对象。文件名只是一个用户友好的引用，实际是指向Inode号的指针。Inode文件系统的几个关键组成部分：
- Indoe表：文件系统中预先分配的区域，用于存储Inode数据结构，每个Inode在Inode表中占用固定大小的空间，通常128或256字节。文件系统在格式化时会预先分配一些Inode，这些Inode数量决定了文件系统所能容纳的最大文件和目录数量。
- 数据块：存储文件内容的基本单元。每个Inode包含了指向文件数据块的指针，这些指针直接指向数据块，也可以指向间接块、二次间接块或三次间接块。
- 目录项：文件系统中表示目录结构的数据结构，每个目录项包含一个文件名和对应的Indoe号。目录项存放在目录文件的数据块中，通过Inode号可以找到目录所指向的文件或子目录的Inode和数据块。
- 超级块（superblock）：文件系统中存储文件系统元数据的区域，包含了文件系统的基本信息，如文件系统类型，大小，块大小，Inode数量等。操作系统在挂载文件系统时会读取超级块，以确定文件系统的参数和状态。


## FAT（File Allocation Table）文件系统和UNIX文件系统
**FAT文件系统**：核心组件是文件分配表，记录了文件数据块的使用情况。

特点如下：
1. 结构简单：以链表的形式管理数据块，随机访问能力较差。
2. 兼容性：大多数操作系统支持FAT文件系统，跨平台兼容性好。
3. 局限性：FAT32不支持超过4GB的单个文件，由于链表顺序访问的特性，大文件性能较差，且没有类似UNIX文件系统的文件权限和所有权管理功能。

适用于以下场景：
1. 可移动存储设备：由于兼容性好，成为U盘、SD卡等的默认文件系统。
2. 嵌入式系统：结构简单、系统资源需求低，适用于嵌入式和低端设备。


**UNIX文件系统**：代指在UNIX和类UNIX操作系统中使用的一系列文件系统，相比于FAT提升了随机访问能力，并且支持链接。

特点如下：
1. Inode结构：使用索引结构来存储文本元数据，管理和访问文件更加高效。
2. 权限管理：支持详细的文件权限和所有权管理，允许对文件和目录进行读、写、执行权限的控制。
3. 硬链接和符号链接
4. 日志功能：许多UNIX文件系统（EXT4, XFS等）支持日志功能，有助于提高文件系统的数据一致性和恢复能力。

适用于以下场景：
1. UNIX和类UNIX操作系统。
2. 服务器和高性能计算：在性能、可靠性、扩展性方面具有优势。


## 文件传输中断后如何续传，Rsync算法
Rsync算法是一种用于文件同步和增量备份的高效算法，可以实现文件传输的续传功能。Rsync算法基于滚动哈希（rolling hash）和数据块签名方法。基本步骤如下：
1. 目标文件分割成数据块，计算两个哈希值：弱哈希值（Adler-32校验和）、强哈希值（MD5, SHA-1）。
2. 将哈希值发送到接收方。接收方在本地查找与目标文件相同的文件，例如旧版本或部分传输的文件。
3. 接收方将本地文件划分为发送方大小相同的数据块，计算每个数据块弱哈希值。
4. 接收方使用滚动哈希方法，在本地文件中寻找与发送方数据块相匹配的哈希值，通过比较弱哈希值，快速找到潜在匹配数据块，并比较强哈希避免误报。
5. 接收方将匹配的数据块信息发送给发送方
6. 发送方根据匹配信息，仅发送目标文件中未匹配的数据块，从而实现增量更新和续传功能。





