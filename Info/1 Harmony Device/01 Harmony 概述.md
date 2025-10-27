---
mindmap-plugin: basic
---

# 概述
## OpenHarmony
- L0(轻量系统)
    - liteos-m 内核
    - 面向 MCU 类处理器
        - 例如 Arm Cortex-M、RISC-V 32 位的设备
        - 硬件资源极其有限，支持 的设备最小内存为 128KiB
        - 可以提供多种轻量级网络协议
        - 轻量级的图形框架
        - 以及丰富 的 IOT 总线读写部件等
- L1(小型系统)
    - liteos-a 内核
    - Linux 内核
    - 面向应用处理器
        - 例如 Arm Cortex-A 的设备
        - 支持的设备最小内存为 1MiB
        - 可以提供更高的安全能力
        - 标准的图形框架、视频编解码的多媒体能力
- L2(标准系统)
    - Linux 内核
    - 面向应用处理器
        - 例如 Arm Cortex-A 的设备
        - 支持的设备最小内存为 128MiB
        - 可以提供增强的交互能力
        - 3D GPU 以及硬件合成能力
        - 更多控件以及动效更丰富的图形能力、完整 的应用框架

## 开发环境
- 概述
    - 主要分为 window、Linux
    - window 环境用于**编写和阅读源代码、烧录系统镜像、抓取日志进行调试**等工作
    - Linux 环境用于**编译源代码、链接和生成可烧录的系统镜像**等工作
- Linux
    - 使用预配置好的Docker环境
    - 开发者自行安装和配置的环境
        - VMware Workstation Pro需要付费使用
        - VMware Workstation Player 是个人免费版本
        - Ubuntu16.04版本以上的64位系统
        - 提供了百度网盘下载linux环境