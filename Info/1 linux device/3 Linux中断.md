---
mindmap-plugin: basic
---

# Linux中断
## 向量中断 和 非向量中断
- 中断发生时，CPU 自动根据中断向量号跳转到对应的中断服务程序
- 中断发生时，跳转到一个固定的中断入口地址，由中断服务程序自行判断中断源并处理

## 中断上下文 和 非中断上下文
- **中断上下文**用于快速响应硬件或软件中断，不能阻塞，执行时间应尽量短。
- **非中断上下文**用于普通任务的执行，可以阻塞，适合处理复杂逻辑或延迟任务。

## 顶半部 和 底半部
- **顶半部**用于快速响应中断，完成紧急任务，保证系统的实时性。
- **底半部**
    - 用于处理复杂任务，降低顶半部的执行时间，提升系统整体性能。
    - 底半部虽然是中断处理的一部分，但它的执行环境已经脱离了中断上下文，因此更准确地说，它是中断处理的延续部分，而不是严格意义上的中断内容。
    - 实现机制
        - **软中断(Softirq)**：高效、不可阻塞，适合高频任务。
        - **Tasklet**：基于软中断，轻量级、不可阻塞，适合简单任务。
        - **工作队列(Workqueue)**：可阻塞，适合复杂或耗时任务。
        - **线程化IRQ（Threaded IRQ）**：线程化，可睡眠或阻塞,适合复杂中断任务。

## 中断编程
- 申请irq
    - request_irq
    - 要申请的硬件中断号
    - 向系统登记的中断处理函数(顶半部)，是一个回调函数 
- 释放irq
    - request_irq
- 使能和屏蔽中断
    - disable_irq 和 enable_irq：用于控制特定中断的启用和禁用。
    - irq_save 和 irq_restore：用于保存和恢复全局中断状态，确保代码段的原子性。
    - local_irq_disable，以local_开头的方法的作用范围是本CPU内
    - disable_irq 和 disable_irq_nosync，disable_irq 会等待指定的中断被处理完再屏蔽

## Linux中断共享
1. 多个设备共享一个IRQ：
   - 使用`request_irq`函数注册中断处理程序，并指定`IRQF_SHARED`标志。
   - 多个设备可以注册到同一个中断号
2. 中断发生时，内核会检查每个中断处理程序
3. 中断处理程序：
   - 必须能够区分是否是自己的设备触发的中断。
   - 必须返回适当的状态（如`IRQ_HANDLED`或`IRQ_NONE`）。
4. 中断共享标志：
   - 注册中断时需要使用`IRQF_SHARED`标志，表明该中断号可以被多个设备共享。

## jiffies
- jiffies 是 Linux 内核中一个全局变量，用于记录系统**自启动以来的时钟中断次数**
- jiffies 的单位是时钟中断（tick），每次时钟中断发生时，jiffies 的值会递增
- jiffies 是一个无符号长整型变量（unsigned long），在 32 位系统上为 4 字节，在 64 位系统上为 8 字节。
- 由于 jiffies 是一个无符号变量，它会在达到最大值后回绕（溢出）为 0。
- 相关宏和函数
    - jiffies：全局变量，记录当前的时钟中断次数
    - msecs_to_jiffies(ms)
    - jiffies_to_msecs
    - time_after 和 time_before
    - schedule_timeout

## 内核定时器
- 定时器的回调函数在软中断上下文中执行，不能阻塞或睡眠
- 内核定时器使用链表管理所有定时器，定时器到期时会触发回调函数。
- 使用
    - 初始化定时器，timer_setup
    - 设置定时器的超时时间和回调函数，mod_timer
    - 启动定时器，add_timer
    - 在不需要时删除定时器，del_timer

## delayed_work
- 是 workqueue 的一种扩展，支持延迟执行
- 通过指定延迟时间（以 jiffies 为单位）来控制任务的执行时间
- 相关函数
    - INIT_DELAYED_WORK(delayed_work, work_handler)：初始化 delayed_work 并绑定处理函数。
    - schedule_delayed_work(delayed_work, delay)：将延迟任务添加到工作队列，delay 为延迟时间（单位为 jiffies）。
    - cancel_delayed_work(delayed_work)：取消未执行的延迟任务。
    - mod_delayed_work(workqueue, delayed_work, delay)：修改延迟时间并重新调度任务 

## 内核延时
- 忙等待（Busy Waiting）
    - 常用函数：udelay 和 ndelay。
    - 特点：精度高，占用 CPU，效率低。
- 睡眠延时（Sleep Delay）  
    - 常用函数：msleep、ssleep 和 usleep_range。
    - 特点：
        - 不占用 CPU。
        - 精度较低，适合毫秒级延时。    