# TWRP Device Configuration File (allducube t1030m Adaptation)

1. Project Description

This device tree is used to compile TWRP Recovery for the allducube t1030m device. However, the current version has functional defects and is only for debugging records during the development phase.

2. Known Issues

Problem Type  Phenomenon Description
Touch Failure  After flashing, the touch screen is unresponsive and the TWRP interface cannot be operated.
Partition Mount Abnormality  Data partition mount fails, resulting in inability to decrypt the partition. Partitions such as system, vendor, metadata, system_dlkm, and vendor_dlkm can already be mounted normally.
USB Function Missing  Cannot connect to the computer via USB (including ADB debugging, file transfer, etc.), but USB mouse is available (hot-swapping not supported).

3. User Guide

Compilation Steps (Android compilation environment needs to be configured in advance)

# Initialize TWRP source code (taking twrp-12.1 branch as an example)
```
repo init -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp.git -b twrp-12.1  
repo sync  

```
# Place this device tree into the device/allducube/t1030m directory  
```
mkdir -p device/allducube/t1030m  
cp -r [your device tree directory] device/allducube/t1030m  

```
# Compile recovery image  
```
source build/envsetup.sh  
lunch 5
ALLOW_MISSING_DEPENDENCIES := true
make vendorbootimage -j$(nproc)  

```

Flashing Notes

1. Be sure to back up the original vendorboot partition before flashing (via dd command or third-party tools).
2. This version is only for development and debugging; do not use it in production environments.

4. Troubleshooting Directions (for developers' reference)

Problem Type  Possible Causes
Touch Failure  - Incorrect configuration of touch driver nodes in the device tree (e.g., input device definition in dts files).  - TWRP fails to load the touch driver module correctly.
Partition Mount Abnormality  - Incorrect configuration of partition paths and file system types in the fstab file.  - Incorrect description of the partition table (e.g., mtd or block nodes) in the device tree.
USB Function Missing  - Driver initialization code for the USB controller is not enabled (kernel or device tree configuration).  - USB power management (e.g., vbus) is not configured.

It may also be that the lk restricts the loading of the touch controller.
Recently, it has been found that there is a high probability that the kernel has a touch disabler, which may require disassembly and patching.
![Underlying Firmware Version](Screenshot_20250627-154451_Chrome.png)

5. Acknowledgments

Thanks to the TWRP community and open-source device tree templates. This project is modified and optimized based on existing codes.

Note: To fix the functions, it is recommended to first compare the normal device tree of the same chip platform, focusing on troubleshooting the code related to driver nodes, partition configuration, and power management.
