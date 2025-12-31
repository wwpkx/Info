# FA
- Feature Ability，代表有界面的元能力，用于与用户进行交互。

# HAP
- HarmonyOS Ability Package
- 一个HAP文件包含应用的所有内容，由代码、资源、三方库及应用配置文件组成
- 其文件后缀名为.hap

# DFX 
- 在图形系统中，DFX 是 Design for eXperience 的缩写
- 是一套面向用户体验的调试和优化工具，帮助开发者提升应用的图形性能和交互体验
- DFX 的功能
    - 截屏功能  
    - 导出组件树  
    - 性能分析  
    - 事件分发调试  
    - 动画调试  
- 适用场景
    - 调试 UI 显示问题。
    - 优化图形渲染性能。
    - 分析组件树结构和事件分发逻辑。
    - 提升应用的用户体验。

# musl libc
## musl libc库简介
- http://musl.libc.org
- **支持 POSIX 和 C标准**，专为**嵌入式系统和资源受限的环境**设计
- **Linux内核部分支持POSIX**
- **边界是系统调用**
- musl实现的文件操作标准
   - POSIX： open、read、write、close 等
   - c标准： fopen、fread、fwrite、fclose等

## musl libc 与其他 C 标准库的对比
1. 与 glibc
   - `glibc` 是 GNU 项目的 C 标准库，功能强大，但体积较大，适合桌面和服务器环境
   - `musl libc` 更轻量，适合嵌入式和资源受限的设备
2. 与 uClibc
   - `uClibc` 也是一个轻量级 C 标准库，但开发已停滞
   - `musl libc` 更现代，支持最新的 POSIX 和 C 标准
3. 与 bionic
   - `bionic` 是 Android 的 C 标准库，专为 Android 系统设计
   - `musl libc` 更通用，适用于多种操作系统

