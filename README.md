# bad_allducube_t1030m_twrp_device_tree
这是一个酷比魔方t1030m的twrp,可以编译通过，但刷进去不开机


酷比魔方T1030M TWRP适配求助：vendor_boot中Recovery卡LOGO重启
 
核心信息
 
- 机型：酷比魔方T1030M
- 系统：安卓12底层刷入安卓14，采用VAB（Vendor Boot）结构
- Recovery配置：无独立recovery分区，TWRP完整资源（二进制、脚本、fstab）打包至 vendor_boot 分区
 
问题现象
 
- 编译： mka vendor_bootimage 成功生成镜像，无报错
- 刷入后：卡开机LOGO约30秒，随后无限重启，无法进入TWRP
- 排除：已用 fastboot --disable-verity 刷入，非签名验证问题
 
vendor_boot关键配置（BoardConfig.mk）
 
BOARD_USES_VENDOR_BOOT := true  # 启用vendor_boot作为Recovery载体
BOARD_MOVE_RECOVERY_RESOURCES_TO_VENDOR_BOOT := true  # 资源移至vendor_boot
TARGET_USES_VAB := true  # 启用VAB结构
TW_RECOVERY_ROOT := $(LOCAL_PATH)/recovery/root  # 资源路径指向vendor_boot内
 
 
求助点
 
需协助排查：
 
1. vendor_boot中Recovery的启动逻辑是否有误（如init脚本未正确加载 /vendor_boot/recovery 资源）？
2. 安卓12底层与安卓14的 vendor_boot 分区是否存在启动兼容性冲突？
3. 无限重启是否因 vendor_boot 与内核/设备树的适配问题（如驱动不支持从vendor_boot启动Recovery）？
 
盼熟悉VAB结构与vendor_boot启动流程的开发者指导排查，感谢！
