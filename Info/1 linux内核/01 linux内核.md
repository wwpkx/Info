---
mindmap-plugin: basic
---

# 内核

## 内核源码来源
- linux内核源码
- 芯片原厂内核源码
- 板卡厂商内核源码

## Kconfig
- **用于软件配置的描述性语言和工具集**
- 通过**依赖关系**确保配置的合理性
- 最终生成一份精简的配置文件（`.config`），指导编译系统只编译选中的功能
- `make xxx_defconfig` 快速加载一套默认配置，避免从零开始配置

## 设备树
- 设备树是Linux内核中​**​硬件描述与驱动解耦​**​的核心技术
- 关键要点
	- ​​文件类型​​：.dts（源文件）、.dtsi（头文件）、.dtb（二进制文件）。
	- ​​语法结构​​
		- 节点`（<device-type>[@<unit-address>]）`
		- 属性`（key=value）`
		- 属性定义`（compatible、reg、interrupts）`
	-  ​工作流程​​
		- 编译（DTC）→ Bootloader 传递 DTB → 解析（内核）→ 驱动匹配（compatible属性）
	- ​​高级特性​​
		- 设备树覆盖（动态修改）
		- 条件属性（动态启用）
		- 多架构支持（RISC-V、ARM64）

## 内核编译
- 设置交叉编译环境
- 更改内核配置，`make menuconfig`
- 编译内核和设备树
	- `make -j4`
	- `make zImage -j4`
	- `make dtbs –j4`
- 编译模块，安装模块，打包模块
    - `make modules -j4`
	- `make modules_install INSTALL_MOD_PATH=./.tmp/rootfs/`
	- `cd .tmp/rootfs/`
	- `tar -jcvf modules.tar.bz2 *`

