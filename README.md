### 配置

| 配置      | 型号                                |
|---------|-----------------------------------|
| CPU     | 12代i5-12400F                      |
| 主板      | 铭瑄B660M挑战者                        |
| 显卡      | 蓝宝石RX6800                         |
| 内存      | 铭瑄32G 3200                        |
| 硬盘      | 三星PM9A1                           |
| 电源      | 长城P600铜牌600W                      |
| 散热器     | 零下三十度无光                           |
| 机箱      | 暗夜猎手5黑色+3日食灯风扇                    |
| 蓝牙+WIFI | BCM94352z+NGFF转PCIE双天线转接板+PCIE延长线 |

### 总结
#### 一、更新RealtekRTL8111.kext后无法获取ip问题：
自定义网卡 MAC 地址：，编辑 RealtekRTL8111.kext/Contents/Info.plist，找到 fallbackMAC 然后把机器底部的 MAC 填进去，格式XX:XX:XX:XX:XX:XX，进入window查看网卡MAC。

#### 二、蓝牙开关可以开启，无法扫描到设备问题：
添加以下 2 项内容
1. NVRAM (7C436110-AB2A-4BBB-A880-FE41995C9F82)
   boot-args -lilubetaall  
2. NVRAM (7C436110-AB2A-4BBB-A880-FE41995C9F82)  
   bluetoothExternalDongleFailed - Data - 00
   bluetoothInternalControllerInfo - Data - 00000000 00000000 00000000 0000
https://www.insanelymac.com/forum/topic/359987-release-macos-sequoia-150/page/4/

#### 三、添加重置Nvram入口方式：
UEFI -> Drivers -> ResetNvramEntry.efi -> Enabled

#### 四、WIFI驱动问题注意：
定制优化总结一下：
1. 禁用AMFI，使用启动参数 amfi=0x80，不使用AMFIPass.kext，否则蓝牙驱动失败。
2. amfi=0x80可能出现部分应用异常和系统检测不到更新等故障问题修复：
   a. revpatch=sbvmm（须配合使用内核事件修补程序 RestrictEvents.kext）
   b. ipc_control_port_options=0
3. 禁用系统完整性保护SIP，使用必须配置03080000，如果设置为00000000，恢复模式csrutil disable没用。
4. 蓝牙需要Reset Nvram生效。
5. 顺序要符合先IOSkywalkFamily、IO80211FamilyLegacy，再其他蓝牙和WIFI的kext
https://bbs.pcbeta.com/viewthread-1975545-1-1.html
https://www.bilibili.com/opus/987828407802789892?spm_id_from=333.1387.0.0

#### 五、隔空投送只能接收不能发送的问题：
前提当然注入三码，退出同一个ID下想互联的设备的ID，清除本机的nvram，然后重新登ID，可以无限制隔空投送和接力。不行再重启就可以了，可能需要iPhone也做一样操作。最好按照下图那样配置一下：
![](README/%E6%88%AA%E5%B1%8F2025-05-04%2019.31.52.png)
#### 六、还有问题未解决的：（已解决的盆友可以留言）
1. 睡眠后无法USB唤醒。
2. iPhone镜像无法连接。

