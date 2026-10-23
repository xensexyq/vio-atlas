# 项目目录

[English](CATALOG.md) · [简体中文](CATALOG-zh.md) · [首页](../README-zh.md)

收录 47 项资源，资料整理日期：2026-10-23。条目区分 VIO、纯视觉 SLAM、同步工具和许可受限的源码方案；不按 star 数量推断性能或产品成熟度。

<a id="vio"></a>

## 1. VIO 与视觉惯性 SLAM

| 项目与源码 | 能解决什么 | 使用边界与集成说明 |
| --- | --- | --- |
| [OpenVINS](https://github.com/rpng/open_vins) | MSCKF 滤波式 VIO，支持单目及多相机、在线时空标定、仿真和评估。 | 适合从爪 PC 端首套基线；官方有 ROS 2 接口，具体发行版需验证。核心里程计与全局回环需区分。GPLv3。 |
| [Basalt](https://gitlab.com/VladyslavUsenko/basalt) | 视觉惯性里程计和建图，配套相机与 IMU 标定、平方根边缘化。 | 适合双目和广角系统的对照；需适配自身配置及标定格式。BSD 3-Clause。 |
| [VINS-Mono](https://github.com/HKUST-Aerial-Robotics/VINS-Mono) | 单目加 IMU，滑窗优化、在线时间偏移估计、回环及滚动快门相关配置。 | 研究软同步和在线标定的经典实现；原版依赖 ROS 1，不能把时间偏移估计理解为任意抖动消除。GPLv3。 |
| [VINS-Fusion](https://github.com/HKUST-Aerial-Robotics/VINS-Fusion) | 单目加 IMU、双目加 IMU、纯双目；在线空间及时间标定、回环和 GPS 融合示例。 | 适合研究多传感器扩展；原版 ROS 1 集成较多，ROS 2 需另核对应实现。GPLv3。 |
| [ORB-SLAM3](https://github.com/UZ-SLAMLab/ORB_SLAM3) | 视觉及视觉惯性 SLAM、多地图、回环与重定位，支持针孔和鱼眼模型。 | 适合全局轨迹和地图复用对照；初始化、标定及现代构建环境需要适配。GPLv3。 |
| [OKVIS2](https://github.com/ethz-mrl/okvis2) | 基于关键帧的视觉惯性 SLAM，带回环，提供 ROS 2 构建及 RealSense 示例。 | 想同时验证 VIO 和完整 SLAM 时优先考虑；配置和依赖比纯里程计多。BSD 3-Clause。 |
| [Kimera-VIO](https://github.com/MIT-SPARK/Kimera-VIO) | 基于优化的视觉惯性状态估计，支持单目或双目，包含回环及网格相关能力。 | 适合连接 Kimera 建图生态；核心库与 ROS 包装层分开核对。BSD 2-Clause。 |
| [XRSLAM](https://github.com/openxrlab/xrslam) | 轻量优化式 VIO，支持桌面和移动平台，并有视觉定位模块。 | 适合嵌入产品接口和跨平台探索；依赖项分别授权，不能只看主库 Apache 2.0。 |
| [DM-VIO](https://github.com/lukasvst/dm-vio) | 基于直接法和延迟边缘化的单目视觉惯性里程计。 | 适合作为算法路线对照；需要准确时序及图像模型，提供独立 ROS 包装仓库。GPLv3。 |
| [AirSLAM](https://github.com/sair-lab/AirSLAM) | 学习式点线特征结合几何优化，支持视觉及视觉惯性模式、地图优化和重定位。 | 适合光照变化场景专项评估；官方环境包含 CUDA、TensorRT 和 ROS Noetic，Jetson 部署需额外工作。GPLv3。 |

<a id="software-sync"></a>

## 2. 软件时间对齐与时钟处理

软件方案可以独立用于原型，也可以与硬件同步配合。VINS-Mono 的在线时间标定、VINS-Fusion 的时空标定及 OpenVINS 的相机与 IMU 时间偏移估计，属于上一节算法内的能力；Kalibr 与 iKalibr 的离线标定见后文。下面补充可拆分复用的时钟和数据处理方案。

| 项目与源码 | 能解决什么 | 使用边界与集成说明 |
| --- | --- | --- |
| [CuckooTimeTranslator](https://github.com/ethz-asl/cuckoo_time_translator) | 将传感器硬件时钟中的事件时间转换到主机时间，参考设备到 PC 的时钟映射实现。 | 当前范围主要是单向通信；原文明确保留未知偏置和噪声，不能单靠它恢复真实曝光延迟。经典 ROS 工程。 |
| [MAVROS 与 MAVLink TIMESYNC](https://github.com/mavlink/mavros) | 通过请求响应、往返时间和滤波估计设备与主机的时间偏移。 | 适合参考 MCU 到 PC 的双向对时协议；不要求产品采用完整飞控系统，也不会自动同步相机曝光。 |
| [Ts Online Optimization](https://github.com/bytedance/Ts_Online_Optimization) | ICRA 2025 在线时间标定研究实现，集成到 VINS 框架中估计视觉惯性时间偏移。 | 适合与 VINS-Mono 默认时间标定做对照；实验环境较旧，应先在记录数据上验证。GPLv3。 |
| [ROS 2 message_filters](https://github.com/ros2/message_filters) | 按消息已有时间戳进行精确或近似配对及缓存处理。 | 适合图像与其他数据的接入层。只做消息配对，不校准时钟、不消除采样时间误差。BSD 3-Clause。 |
| [LinuxPTP](https://www.linuxptp.org/) | Linux 下的 IEEE 1588 PTP 实现，用于网络设备及硬件时钟的同步。 | 适合支持 PTP 的 GigE 相机、网卡和多机系统；普通 USB 相机不能仅安装软件就获得曝光硬同步。 |
| [chrony](https://chrony-project.org/) | NTP 及参考时钟支持，用于 PC 系统时钟同步和监控。 | 适合多主机记录及时间基准管理；不替代相机与 IMU 的采样事件时间标记。GPLv2。 |

MAVLink 的 [TIMESYNC 协议说明](https://mavlink.io/en/services/timesync.html)给出了请求响应、往返时间及滤波过程。CuckooTimeTranslator 原文明确说明单向映射仍有未知偏置；它与利用运动观测估计相机和 IMU 时间偏移是互补关系。message_filters 的近似配对只是放宽已有时间戳的匹配窗口，不能把错误的采样时间变正确。

<a id="hardware-sync"></a>

## 3. 硬件同步平台与参考工程

以下十组工程分别覆盖完整相机与 IMU 平台、曝光反馈采集、STM32 或 Teensy 实现，以及 LiDAR 扩展。含雷达的工程主要借鉴计时、触发和驱动处理，不能直接视为从爪 VIO 成品。

| 项目与源码 | 能解决什么 | 使用边界与集成说明 |
| --- | --- | --- |
| [VersaVIS](https://github.com/ethz-asl/versavis) | 相机加 IMU 完整同步平台，包含固件、PC 软件、相机触发、曝光补偿及主机时钟转换。 | 首选完整架构参考；另有 versavis_hw 硬件文档和 BOM。原版 ROS 1，支持的相机和 IMU 需要逐项移植。主软件 BSD 3-Clause。 |
| [Visual Inertial Synchronization Hardware](https://github.com/TurtleZhong/Visual-Inertial-Synchronization-Hardware) | Arduino Mega、MPU6050、外触发 Bumblebee2；发送 IMU 时间戳及触发计数，由 PC 匹配图像。 | 适合理解最小系统和驱动配对；原环境 Ubuntu 16.04 与 ROS Kinetic。公开参考工程，代码复用前逐文件核对授权。 |
| [vio_hardware](https://github.com/tim-fan/vio_hardware) | Teensy 3.2、Basler 相机和 MPU6050；通过相机 Exposure Active 输出记录帧时间。 | 适合相机自由运行、MCU 采集曝光反馈的路线。原型工程较老，IMU 时间标记语义和授权需另核。 |
| [Robosense Xsens Hikvision Hardware Sync](https://github.com/LC-Robotics/Robosense-Xsens-Hikvision-Hardware-Sync) | 包含 PCB、CAD、STM32 同步工程和驱动；以 Xsens 时钟串联 LiDAR 与相机触发。 | 适合 STM32、电气连接和驱动补偿参考；绑定 MTi-300、Helios-32、特定海康相机，不能原样套用。BSD 2-Clause。 |
| [LIV_handhold](https://github.com/xuankuzcr/LIV_handhold) | FAST-LIVO 系列手持及无人机传感器装置，提供 STM32 定时同步和修改后的相机、Livox 驱动。 | 适合研究 PPS、相机触发、数据时间戳的一体实现；含 stm32_timersync-open。主仓库 GPLv3。 |
| [LIV Eye](https://github.com/hku-mars/LIV_handhold_2) | LiDAR 惯性视觉采集套件，包含硬件清单、同步连接、驱动、标定和 FAST-LIVO2 工作流。 | 可参考系统集成和复现流程；部分配置使用采购同步器，也链接 LIV_handhold 的 STM32 方案。GPLv2。 |
| [LiDAR Sync Mimics GPS](https://github.com/Marselka/lidar-sync-mimics-gps) | STM32F4、MPU9150、Velodyne，通过模拟 GNSS 时钟接口统一 LiDAR 与 IMU 时序。 | 主要是 LiDAR 与 IMU 的扩展参考，不是现成相机同步板。原组织仓库已归档；作者仓库可读，复用授权另核。 |
| [Hardware Trigger Time Sync LiDAR Cameras](https://github.com/shiva-THI/hardware-trigger-time-sync-lidar-cameras) | 公开 KiCad 原理图、PCB 及 Arduino 延迟控制代码；包含 LiDAR 配三相机和七相机设计。 | 适合触发分配、电路设计和多相机扩展；不是完整相机加 IMU VIO 系统。GPLv3。 |
| [hard_sync_dut](https://github.com/ZhuangYanDLUT/hard_sync_dut) | 3D-M-Box 多传感器同步驱动，处理 PPS、传输延迟、队列和样本索引关联。 | 适合借鉴丢事件、跨节点延迟和序号关联处理；以特定多传感器平台为前提。GPLv3。 |
| [OctoSense](https://github.com/anthonytec2/OctoSense) | ROS 2 Jazzy 多传感器采集栈，Teensy 4.1 PPS 固件、KiCad SyncBoard、录包及健康监控。 | 适合较新 ROS 2 数据采集流程参考；传感器及网络配置需替换，部分事件相机需要专有 HAL 插件。MIT。 |

VersaVIS 的 [硬件与引脚资料](https://github.com/ethz-asl/versavis_hw)单独提供相机 Trigger、Exposure 和 IMU Data Ready 相关连接。LiDAR Sync Mimics GPS 的[原组织仓库](https://github.com/MobileRoboticsSkoltech/lidar-sync-mimics-gps)已归档，与作者仓库作为同一项目家族收录。

<a id="drivers"></a>

## 4. 传感器驱动与采集 SDK

| 项目与源码 | 能解决什么 | 使用边界与集成说明 |
| --- | --- | --- |
| [librealsense](https://github.com/realsenseai/librealsense) | RealSense 开源 SDK，提供图像、运动数据及时间戳相关接口。 | 作为成品传感器接入和时间戳语义的参考；跨流、跨设备同步支持取决于具体型号，不能由同步深度图推断 IMU 也已同步。 |
| [ZED Open Capture](https://github.com/stereolabs/zed-open-capture) | 无需 CUDA 的底层 C++ 图像及传感器采集库，包含图像与 IMU 关联示例。 | 适合参考 USB 视频和 HID 运动数据接入。支持 ZED 2、2i、Mini 等 USB 型号，不适用于 ZED X 系列 GMSL2 型号；不是完整开源定位引擎。MIT。 |
| [DepthAI Core](https://github.com/luxonis/depthai-core) | OAK 设备的 C++ 主机库与数据管线接口，配套设备时间戳和主机时钟映射机制。 | 适合研究设备端时间戳、消息序号和主机同步；设备固件及传感器能力需分别核对，主机库开源不等于整套硬件固件完全开放。 |
| [FLIR Camera Driver](https://github.com/ros-drivers/flir_camera_driver) | ROS 驱动，支持传感器时间戳转换、PTP 配置及硬触发多相机的配对。 | 重点看 spinnaker_camera_driver 与 synchronized 子包；依赖厂商 Spinnaker SDK。PTP 仅适用于支持它的型号，默认收帧时间戳不等于曝光时间。 |
| [Aravis](https://github.com/AravisProject/aravis) | GenICam 相机采集库，覆盖 GigE Vision 和 USB3 Vision。 | 适合不采用特定 ROS 包时构建 PC 采集层；触发、时间戳及 PTP 节点由相机实现决定。LGPL 2.1 或更新版本。 |
| [libcamera](https://libcamera.org/) | Linux 相机管线框架，定义 SensorTimestamp、ExposureTime 等控制及元数据。 | 适合 CSI 及嵌入式相机集成。实际时间戳由平台驱动实现；不是对任意 UVC 摄像头提供同步升级。 |

驱动是软件同步能否落地的关键。FLIR 驱动的[时间戳与 PTP 说明](https://github.com/ros-drivers/flir_camera_driver/blob/humble-devel/spinnaker_camera_driver/doc/index.rst)区分 SDK 交付时间、传感器时间戳及 PTP。libcamera 的[控制元数据定义](https://docs.libcamera.org/master/public-api/control__ids_8h.html)说明 SensorTimestamp 对应首行曝光时刻；具体平台仍须确认驱动实现。DepthAI 的[设备对时说明](https://docs.luxonis.com/software-v3/ai-inference/conversion/rvc-conversion/offline/)可用于参考设备到主机的持续时钟映射。

<a id="calibration"></a>

## 5. 空间与时间标定

| 项目与源码 | 能解决什么 | 使用边界与集成说明 |
| --- | --- | --- |
| [Kalibr](https://github.com/ethz-asl/kalibr) | 相机、相机与 IMU、多 IMU 及滚动快门相关标定；可估计相机与 IMU 时间偏移。 | 离线标定首选基线；需有效激励、清晰标定板和低抖动时间戳，不能补回丢失样本或随机传输延迟。 |
| [iKalibr](https://github.com/Unsigned-Long/iKalibr) | 连续时间的无靶标多传感器时空标定，支持 IMU、相机、RGB-D、LiDAR 和雷达等组合。 | 适合自然场景及复杂传感器组合；是独立框架而非 Kalibr 插件，需要充分运动激励。许可证按根 LICENSE 与依赖分别核对。 |
| [Allan Variance ROS](https://github.com/ori-drs/allan_variance_ros) | 由 IMU 数据生成 Allan 偏差分析和噪声参数。 | 用于构建 VIO 所需的噪声模型；是静态噪声分析，不是时间同步工具。BSD 3-Clause。 |
| [imu_utils](https://github.com/gaowenliang/imu_utils) | IMU 性能及 Allan 方差分析工具，提供噪声相关参数估计。 | 另一套常用噪声分析路径；核对采样率、单位和原版依赖。MIT。 |
| [IMU TK](https://github.com/Kyle-ak/imu_tk) | 低成本 MEMS IMU 的内参标定、数据处理和显示工具。 | 适合偏置、尺度和轴间误差相关研究；原说明测试环境较老，需处理现代依赖兼容性。README 声明 BSD。 |

Kalibr 的[相机与 IMU 标定说明](https://github.com/ethz-asl/kalibr/wiki/camera-imu-calibration)要求同一时钟下的低抖动时间戳及充分运动激励。标定相机内参、相机与 IMU 外参之后，还要测量传感器到夹爪 TCP 的完整变换；CAD 安装位置不能直接代替实测标定。

<a id="evaluation"></a>

## 6. 轨迹评估与可回放记录

| 项目与源码 | 能解决什么 | 使用边界与集成说明 |
| --- | --- | --- |
| [evo](https://github.com/MichaelGrupp/evo) | 里程计及 SLAM 的轨迹评估和绘图，支持 APE、RPE 等指标。 | 比较 VIO、机械臂 FK 与外部参考时统一坐标、时间及对齐方式；有米制尺度的 VIO 不应随意用尺度拟合掩盖误差。GPLv3。 |
| [rosbag2](https://github.com/ros2/rosbag2) | ROS 2 话题记录与回放，可结合 MCAP 存储。 | 用于同一份原始数据反复比较算法；同时保留源时间、接收时间、序号和重置事件。Apache 2.0。 |
| [MCAP](https://github.com/foxglove/mcap) | 面向机器人及发布订阅数据的容器格式，配套多语言读写库。 | 适合自有 SDK 的多模态数据落盘与离线分析；文件格式本身不会实现传感器同步。MIT。 |

<a id="mapping"></a>

## 7. 多传感器和稠密建图扩展

这些项目用于能力扩展或算法对照，其中纯视觉 SLAM 不应标为 VIO。稠密地图、开放词汇查询和回环能力，也不等同于高频、连续且低延迟的控制反馈。

| 项目与源码 | 能解决什么 | 使用边界与集成说明 |
| --- | --- | --- |
| [OKVIS2 X](https://github.com/ethz-mrl/OKVIS2-X) | 视觉惯性 SLAM，可配置深度、LiDAR 和 GNSS，构建体积占据子地图，并包含 FindAnything 相关模式。 | 定位走向探索、导航及开放词汇建图的扩展；不同模式依赖不同，不是 VIO 最小部署。BSD 3-Clause。 |
| [FAST LIVO2](https://github.com/hku-mars/FAST-LIVO2) | LiDAR、惯性与视觉融合里程计和建图。 | 有雷达时值得评估；不能按单鱼眼加 IMU 直接运行，配套同步设计可参考 LIV_handhold。GPLv2。 |
| [DROID SLAM](https://github.com/princeton-vl/DROID-SLAM) | 学习式稠密视觉 SLAM，支持单目、双目及 RGB-D。 | 作为视觉路线对照；原版不是 VIO，不直接融合 IMU，需 GPU。BSD 3-Clause。 |
| [VIGS SLAM](https://github.com/cvg/VIGS-SLAM) | RGB 加 IMU，轨迹估计与 3D Gaussian 地图重建结合。 | 属于视觉惯性稠密重建研究路线，GPU 及环境依赖较重。主项目 Apache 2.0，但部分第三方组件许可更严格。 |
| [VGGT SLAM 2.0](https://github.com/MIT-SPARK/VGGT-SLAM) | 基础模型驱动的稠密视觉重建、回环及可选开放词汇物体检测。 | 原版不是 VIO；地图更新速率不能直接等同于控制所需位姿频率。主代码 BSD 2-Clause，模型和可选组件分别授权。 |
| [MASt3R SLAM](https://github.com/rmurai0610/MASt3R-SLAM) | 基于三维重建先验的稠密视觉 SLAM。 | 源码可获取的研究参考，原版不直接融合 IMU；采用 CC BY-NC-SA 4.0，含非商业限制，单独标注。 |

<a id="restricted-source"></a>

## 8. 平台受限的源码方案

| 项目与源码 | 能解决什么 | 使用边界与集成说明 |
| --- | --- | --- |
| [cuVSLAM](https://github.com/nvidia-isaac/cuVSLAM) | NVIDIA 提供源码构建、C++ 与 Python API，以及 ROS 2 接入；包含双目惯性模式。 | 适合 NVIDIA GPU 和 Jetson 路线。采用限定 NVIDIA 平台的 NVIDIA Community License，单独列为源码可获取方案，不归入无平台限制的开源许可。 |

cuVSLAM 的[许可证](https://github.com/nvidia-isaac/cuVSLAM/blob/main/LICENSE)限定 NVIDIA 平台；MASt3R-SLAM 的[许可证](https://github.com/rmurai0610/MASt3R-SLAM/blob/main/LICENSE.md)含非商业限制。VIGS-SLAM 与 VGGT-SLAM 还需要分别检查第三方组件及模型权重许可。软件、硬件设计、相机厂商 SDK 和模型权重不能默认沿用同一个授权。
