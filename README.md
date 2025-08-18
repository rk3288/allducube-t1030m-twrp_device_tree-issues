TWRP 设备配置文件（allducube t1030m 适配）
 
1. 项目说明
 
本设备树用于为 allducube t1030m 设备编译 TWRP Recovery，但当前版本存在功能缺陷，仅作为开发阶段的调试记录。
 
2. 已知问题
 
问题类型 现象描述 
触摸失效 刷入后触摸屏无响应，无法操作 TWRP 界面。 
分区挂载异常  system 、 vendor 、 data  等多数分区无法挂载，导致无法读写分区内容。 
USB 功能缺失 无法通过 USB 连接电脑（包括 ADB 调试、文件传输等功能均失效）。 
 
3. 使用指南
 
编译步骤（需提前配置 Android 编译环境）
 
# 初始化 TWRP 源码（以 twrp-12.1 分支为例）
```bash
repo init -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp.git -b twrp-12.1  
repo sync  

```
# 将本设备树放入 device/allcube/t1030m 目录  
```bash
mkdir -p device/allducube/t1030m  
cp -r [你的设备树目录] device/allducube/t1030m  

```
# 编译 recovery 镜像  
```bash
source build/envsetup.sh  
lunch 5 
make vendorbootimage -j$(nproc)  
 
 
 ```
刷入提示
 
1. 刷入前务必备份原有 Recovery 分区（可通过  fastboot backup  或第三方工具）。
2. 本版本仅适用于 开发调试，请勿用于生产环境。
 
4. 问题排查方向（供开发者参考）
 
问题类型 可能原因 
触摸失效 - 设备树中 触摸驱动节点 配置错误（如  dts  文件的  input  设备定义）。   - TWRP 未正确加载触摸驱动模块。 
分区挂载异常 -  fstab  文件的 分区路径、文件系统类型 配置错误。   - 设备树中分区表（如  mtd  或  block  节点）描述错误。 
USB 功能缺失 - USB 控制器的 驱动初始化代码 未启用（内核或设备树配置）。   - USB 电源管理（如  vbus  使能）未配置。 
也有可能是lk限制了触摸控制器加载
 
5. 致谢
 
感谢 TWRP 社区及开源设备树模板，本项目基于已有代码修改优化。
 
注意：若需修复功能，建议优先对比同芯片平台的正常设备树，重点排查 驱动节点、分区配置、电源管理 相关代码。