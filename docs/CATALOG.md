# Project catalog

[English](CATALOG.md) · [简体中文](CATALOG-zh.md) · [Home](../README.md)

47 resources, reviewed on 2026-10-23. Entries distinguish VIO, visual-only SLAM, synchronization tools and source-available software with license restrictions. Stars do not establish performance or production readiness.

<a id="vio"></a>

## 1. VIO and visual-inertial SLAM

| Project and source | Purpose | Boundaries and integration |
| --- | --- | --- |
| [OpenVINS](https://github.com/rpng/open_vins) | MSCKF-based VIO with monocular/multi-camera operation, online spatial and temporal calibration, simulation and evaluation. | A useful first PC-side follower-gripper baseline. An official ROS 2 interface exists; verify the target distribution. Distinguish odometry from global loop closure. GPLv3. |
| [Basalt](https://gitlab.com/VladyslavUsenko/basalt) | Visual-inertial odometry and mapping, camera/IMU calibration and square-root marginalization. | A comparison candidate for stereo and wide-angle setups; adapt configuration and calibration formats. BSD 3-Clause. |
| [VINS-Mono](https://github.com/HKUST-Aerial-Robotics/VINS-Mono) | Monocular-inertial sliding-window optimization, online time-offset estimation, loop closure and rolling-shutter settings. | A classic temporal-calibration reference. The original uses ROS 1; estimating a time offset does not remove arbitrary jitter. GPLv3. |
| [VINS-Fusion](https://github.com/HKUST-Aerial-Robotics/VINS-Fusion) | Monocular-inertial, stereo-inertial and stereo-only estimation, online spatial/temporal calibration, loop closure and a GPS example. | Useful for multi-sensor studies. The original integration is largely ROS 1; assess ROS 2 implementations separately. GPLv3. |
| [ORB-SLAM3](https://github.com/UZ-SLAMLab/ORB_SLAM3) | Visual and visual-inertial SLAM, multiple maps, loop closure and relocalization with pinhole/fisheye models. | Useful for global trajectories and map reuse. Initialization, calibration and modern build environments need attention. GPLv3. |
| [OKVIS2](https://github.com/ethz-mrl/okvis2) | Keyframe-based visual-inertial SLAM with loop closure, ROS 2 build instructions and RealSense examples. | A candidate when evaluating both VIO and full SLAM; more configuration and dependencies than a minimal odometry stack. BSD 3-Clause. |
| [Kimera-VIO](https://github.com/MIT-SPARK/Kimera-VIO) | Optimization-based monocular/stereo visual-inertial estimation with loop-closure and mesh-related capabilities. | Useful for the Kimera mapping ecosystem; check the core library and ROS wrappers separately. BSD 2-Clause. |
| [XRSLAM](https://github.com/openxrlab/xrslam) | Lightweight optimization-based VIO for desktop/mobile platforms, plus visual localization. | Useful for product interfaces and cross-platform exploration. Check dependency licenses separately from the Apache 2.0 main library. |
| [DM-VIO](https://github.com/lukasvst/dm-vio) | Direct monocular visual-inertial odometry with delayed marginalization. | An alternative algorithmic baseline. Requires sound timing and image models; a separate ROS wrapper is available. GPLv3. |
| [AirSLAM](https://github.com/sair-lab/AirSLAM) | Learned point/line features with geometric optimization, visual/visual-inertial modes, map optimization and relocalization. | Evaluate specifically under lighting changes. The official environment includes CUDA, TensorRT and ROS Noetic; Jetson deployment needs additional work. GPLv3. |

<a id="software-sync"></a>

## 2. Software time alignment and clock handling

| Project and source | Purpose | Boundaries and integration |
| --- | --- | --- |
| [CuckooTimeTranslator](https://github.com/ethz-asl/cuckoo_time_translator) | Maps events from a sensor hardware clock into host time. | A reference for device-to-PC clock translation over one-way communication. Unknown bias and noise remain; this alone cannot recover actual exposure latency. A legacy ROS project. |
| [MAVROS 与 MAVLink TIMESYNC](https://github.com/mavlink/mavros) | Estimates device/host time offset through request/response exchanges, round-trip time and filtering. | A reference for an MCU-to-PC two-way synchronization protocol, without requiring a full flight-control stack. It does not synchronize camera exposure automatically. |
| [Ts Online Optimization](https://github.com/bytedance/Ts_Online_Optimization) | ICRA 2025 implementation of online visual-inertial temporal calibration within VINS. | Compare with the default VINS-Mono estimator on recorded data first; the documented environment is old. GPLv3. |
| [ROS 2 message_filters](https://github.com/ros2/message_filters) | Exact/approximate pairing and buffering using existing message timestamps. | Useful at the ingestion layer. It does not calibrate clocks or correct sampling-time errors. BSD 3-Clause. |
| [LinuxPTP](https://www.linuxptp.org/) | Linux IEEE 1588 PTP implementation for network devices and hardware clocks. | For PTP-capable GigE cameras, NICs and multi-host setups. Installing it does not add hardware exposure synchronization to an ordinary USB camera. |
| [chrony](https://chrony-project.org/) | NTP and reference-clock support for PC system-clock synchronization and monitoring. | Useful for multi-host recording and time-reference management; does not replace camera/IMU measurement-event timestamps. GPLv2. |

Software approaches can support prototypes independently or complement hardware synchronization. Online camera/IMU time-offset estimation is also part of OpenVINS, VINS-Mono and VINS-Fusion; offline calibration appears below.

The official [MAVLink TIMESYNC protocol](https://mavlink.io/en/services/timesync.html) describes exchanges, round-trip-time handling and filtering. One-way CuckooTimeTranslator mapping still retains unknown bias and noise; it complements motion-based camera/IMU time-offset estimation. Approximate message_filters pairing only widens a timestamp-matching window and cannot correct wrong sampling times.

<a id="hardware-sync"></a>

## 3. Hardware synchronization platforms and references

| Project and source | Purpose | Boundaries and integration |
| --- | --- | --- |
| [VersaVIS](https://github.com/ethz-asl/versavis) | Complete camera/IMU synchronization platform with firmware, host software, camera triggering, exposure compensation and host-clock translation. | A primary architecture reference. Separate versavis_hw documentation includes hardware and BOM information. Original ROS 1 integration; port camera/IMU support explicitly. Main software: BSD 3-Clause. |
| [Visual Inertial Synchronization Hardware](https://github.com/TurtleZhong/Visual-Inertial-Synchronization-Hardware) | Arduino Mega, MPU6050 and externally triggered Bumblebee2; sends IMU timestamps and trigger counts for host image association. | Useful for understanding a minimal system and driver pairing. Original Ubuntu 16.04/ROS Kinetic environment. Public reference code; check file-level reuse permissions. |
| [vio_hardware](https://github.com/tim-fan/vio_hardware) | Teensy 3.2, Basler camera and MPU6050; records camera Exposure Active events. | A reference for free-running cameras with MCU exposure-feedback capture. An older prototype; verify IMU timestamp semantics and licensing. |
| [Robosense Xsens Hikvision Hardware Sync](https://github.com/LC-Robotics/Robosense-Xsens-Hikvision-Hardware-Sync) | PCB, CAD, STM32 synchronization code and drivers; Xsens timing connects LiDAR and camera triggers. | Useful for STM32 wiring and driver compensation. Bound to MTi-300, Helios-32 and a specific Hikvision camera; not a drop-in design. BSD 2-Clause. |
| [LIV_handhold](https://github.com/xuankuzcr/LIV_handhold) | FAST-LIVO handheld/UAV sensor setup with STM32 timing synchronization and modified camera/Livox drivers. | A reference for PPS, camera triggering and timestamps together. Includes stm32_timersync-open. Main repository: GPLv3. |
| [LIV Eye](https://github.com/hku-mars/LIV_handhold_2) | LiDAR-inertial-visual capture kit covering hardware lists, synchronization wiring, drivers, calibration and FAST-LIVO2 workflows. | Useful for reproducible system integration. Some configurations use a purchased synchronizer; it also links the LIV_handhold STM32 approach. GPLv2. |
| [LiDAR Sync Mimics GPS](https://github.com/Marselka/lidar-sync-mimics-gps) | STM32F4, MPU9150 and Velodyne synchronization through an emulated GNSS clock interface. | A LiDAR/IMU extension reference, not a ready-made camera synchronization board. The original organization repository is archived; check reuse permissions in the author repository. |
| [Hardware Trigger Time Sync LiDAR Cameras](https://github.com/shiva-THI/hardware-trigger-time-sync-lidar-cameras) | KiCad schematics/PCB and Arduino delay-control code, including LiDAR with three-camera and seven-camera designs. | Useful for trigger distribution, circuitry and multi-camera expansion; not a complete camera/IMU VIO system. GPLv3. |
| [hard_sync_dut](https://github.com/ZhuangYanDLUT/hard_sync_dut) | 3D-M-Box multi-sensor synchronization driver handling PPS, transport latency, queues and sample-index association. | A reference for missing events, cross-node delays and index matching on a specific sensor platform. GPLv3. |
| [OctoSense](https://github.com/anthonytec2/OctoSense) | ROS 2 Jazzy acquisition stack with Teensy 4.1 PPS firmware, KiCad SyncBoard, recording and health monitoring. | A newer ROS 2 acquisition reference; adapt sensors and network configuration. Some event cameras require proprietary HAL plugins. MIT. |

These projects cover full camera/IMU systems, exposure-feedback capture, STM32/Teensy implementations and LiDAR extensions. LiDAR-oriented projects are references for timing, triggers and drivers, rather than finished follower-gripper VIO products.

See the separate [VersaVIS hardware and pin documentation](https://github.com/ethz-asl/versavis_hw). The archived [original LiDAR Sync Mimics GPS repository](https://github.com/MobileRoboticsSkoltech/lidar-sync-mimics-gps) and the author repository are one project family.

<a id="drivers"></a>

## 4. Sensor drivers and acquisition SDKs

| Project and source | Purpose | Boundaries and integration |
| --- | --- | --- |
| [librealsense](https://github.com/realsenseai/librealsense) | Open-source RealSense SDK exposing images, motion data and timestamp interfaces. | A reference for ready-made sensor integration and timestamp semantics. Stream/device synchronization varies by model; synchronized depth does not establish synchronized IMU measurements. |
| [ZED Open Capture](https://github.com/stereolabs/zed-open-capture) | Low-level C++ image/sensor capture without CUDA, including an image/IMU association example. | A reference for USB video plus HID motion data. Supports USB ZED 2/2i/Mini models, not ZED X GMSL2 models. This is not a complete open-source localization engine. MIT. |
| [DepthAI Core](https://github.com/luxonis/depthai-core) | OAK C++ host library and data pipelines with device timestamps and host-clock mapping. | Useful for device timestamps, sequence numbers and host synchronization. Inspect firmware and sensor capabilities separately; an open host library does not make all firmware/hardware open. |
| [FLIR Camera Driver](https://github.com/ros-drivers/flir_camera_driver) | ROS camera drivers with sensor-time conversion, PTP configuration and hard-trigger multi-camera pairing. | Inspect spinnaker_camera_driver and the synchronized subpackage. Depends on vendor Spinnaker SDK. PTP requires a supporting model; default delivery time is not exposure time. |
| [Aravis](https://github.com/AravisProject/aravis) | GenICam acquisition library covering GigE Vision and USB3 Vision. | Useful for a PC capture layer outside a particular ROS package. Trigger, timestamp and PTP nodes depend on the camera. LGPL 2.1 or later. |
| [libcamera](https://libcamera.org/) | Linux camera pipeline framework defining controls/metadata such as SensorTimestamp and ExposureTime. | Useful for CSI and embedded cameras. Actual timestamps depend on platform drivers; it does not upgrade arbitrary UVC cameras to synchronized capture. |

The acquisition driver determines which timestamps are actually available. The [FLIR timestamp/PTP documentation](https://github.com/ros-drivers/flir_camera_driver/blob/humble-devel/spinnaker_camera_driver/doc/index.rst) distinguishes delivery time, device-time conversion and IEEE 1588 modes. [libcamera control metadata](https://docs.libcamera.org/master/public-api/control__ids_8h.html) defines SensorTimestamp at first-row exposure; verify the implementation on the target platform. The [DepthAI device synchronization documentation](https://docs.luxonis.com/software-v3/ai-inference/conversion/rvc-conversion/offline/) is a reference for continuous device-to-host clock mapping.

<a id="calibration"></a>

## 5. Spatial and temporal calibration

| Project and source | Purpose | Boundaries and integration |
| --- | --- | --- |
| [Kalibr](https://github.com/ethz-asl/kalibr) | Camera, camera/IMU, multi-IMU and rolling-shutter calibration, including camera/IMU time-offset estimation. | A baseline for offline calibration. Requires sufficient excitation, clear targets and low-jitter timestamps; cannot recover missing samples or random transport delays. |
| [iKalibr](https://github.com/Unsigned-Long/iKalibr) | Continuous-time, targetless multi-sensor spatial/temporal calibration for IMUs, cameras, RGB-D, LiDAR and radar. | Useful for natural scenes and complex sensor combinations. An independent framework, not a Kalibr plugin; sufficient excitation is required. Check the root LICENSE and dependencies individually. |
| [Allan Variance ROS](https://github.com/ori-drs/allan_variance_ros) | Allan-deviation analysis and noise parameters from IMU recordings. | For VIO noise models; static noise analysis is not time synchronization. BSD 3-Clause. |
| [imu_utils](https://github.com/gaowenliang/imu_utils) | IMU performance and Allan-variance analysis with noise-parameter estimation. | Another common noise-analysis route. Verify sampling rate, units and original dependencies. MIT. |
| [IMU TK](https://github.com/Kyle-ak/imu_tk) | Intrinsic calibration, processing and visualization tools for low-cost MEMS IMUs. | Useful for bias, scale and axis-error studies. The documented environment is old; modern dependencies need adaptation. README declares BSD. |

The [Kalibr camera/IMU calibration guide](https://github.com/ethz-asl/kalibr/wiki/camera-imu-calibration) calls for low-jitter timestamps on a common clock and sufficient excitation. Camera intrinsics, camera/IMU extrinsics, time offset and IMU noise remain separate calibration responsibilities. Also measure the complete sensor-to-gripper-TCP transform; a CAD mounting position does not replace measured calibration.

<a id="evaluation"></a>

## 6. Trajectory evaluation and replayable recording

| Project and source | Purpose | Boundaries and integration |
| --- | --- | --- |
| [evo](https://github.com/MichaelGrupp/evo) | Odometry/SLAM trajectory evaluation and plotting, including APE and RPE. | Unify frames, time and alignment when comparing VIO, robot FK and external references. Do not hide metric-VIO scale errors through arbitrary scale fitting. GPLv3. |
| [rosbag2](https://github.com/ros2/rosbag2) | ROS 2 recording and replay, optionally with MCAP storage. | Reuse the same raw recording across algorithms. Preserve source/receive times, sequence numbers and reset events. Apache 2.0. |
| [MCAP](https://github.com/foxglove/mcap) | Robotics/pub-sub data container format with multi-language readers and writers. | Useful for custom-SDK multimodal recording and offline analysis. A storage format does not synchronize sensors. MIT. |

<a id="mapping"></a>

## 7. Multi-sensor and dense-mapping extensions

| Project and source | Purpose | Boundaries and integration |
| --- | --- | --- |
| [OKVIS2 X](https://github.com/ethz-mrl/OKVIS2-X) | Visual-inertial SLAM with optional depth, LiDAR and GNSS, volumetric occupancy submaps and FindAnything-related modes. | An extension toward exploration, navigation and open-vocabulary mapping. Dependencies vary by mode; not a minimal VIO deployment. BSD 3-Clause. |
| [FAST LIVO2](https://github.com/hku-mars/FAST-LIVO2) | LiDAR-inertial-visual odometry and mapping. | Evaluate when LiDAR is available; cannot run directly from just one fisheye camera and an IMU. See LIV_handhold for synchronization design. GPLv2. |
| [DROID SLAM](https://github.com/princeton-vl/DROID-SLAM) | Learned dense visual SLAM for monocular, stereo and RGB-D inputs. | A visual baseline, not VIO: the original does not fuse IMU measurements. Requires a GPU. BSD 3-Clause. |
| [VIGS SLAM](https://github.com/cvg/VIGS-SLAM) | RGB/IMU trajectory estimation with 3D Gaussian map reconstruction. | A visual-inertial dense-reconstruction research route with substantial GPU/environment dependencies. Main project: Apache 2.0; some third-party components have stricter licenses. |
| [VGGT SLAM 2.0](https://github.com/MIT-SPARK/VGGT-SLAM) | Foundation-model-based dense visual reconstruction, loop closure and optional open-vocabulary object detection. | The original is not VIO. Map-update rate is not equivalent to control-pose frequency. Main code: BSD 2-Clause; models and optional components have separate licenses. |
| [MASt3R SLAM](https://github.com/rmurai0610/MASt3R-SLAM) | Dense visual SLAM based on 3D reconstruction priors. | Source-available research reference; the original does not directly fuse IMU data. CC BY-NC-SA 4.0 includes a noncommercial restriction and is identified separately. |

These are extensions or comparison routes. Visual-only SLAM is not VIO. Dense mapping, open-vocabulary queries and loop closure do not establish continuous, high-rate, low-latency control feedback.

<a id="restricted-source"></a>

## 8. Platform-restricted source-available software

| Project and source | Purpose | Boundaries and integration |
| --- | --- | --- |
| [cuVSLAM](https://github.com/nvidia-isaac/cuVSLAM) | NVIDIA source builds, C++/Python APIs and ROS 2 integration, including stereo-inertial operation. | For NVIDIA GPU/Jetson deployments. The NVIDIA Community License restricts platforms; listed as source-available, not unrestricted open source. |

The [cuVSLAM license](https://github.com/nvidia-isaac/cuVSLAM/blob/main/LICENSE) restricts use to NVIDIA platforms. [MASt3R-SLAM licensing](https://github.com/rmurai0610/MASt3R-SLAM/blob/main/LICENSE.md) has a noncommercial restriction; do not treat it as an unrestricted commercial dependency. Check third-party components and model weights for VIGS-SLAM and VGGT-SLAM separately. Software, hardware designs, camera vendor SDKs and model weights do not automatically share one license.
