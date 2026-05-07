# BoomBoomFly

BoomBoomFly 是一个围绕 PX4 无人机、ROS 2 和 DDS 通信链路整理的工程集合，主要面向小型无人机实机调试、比赛流程启动、视觉定位接入、Offboard 底层控制和上下位机通信。

当前项目基于 Ubuntu 20.04 与 ROS 2 Foxy，重点维护从依赖安装、工作区构建、PX4 DDS 链路、T265 视觉里程计、串口通信到完整启动流程的一组可复用工具和功能包。

## 我们在做什么

- 搭建 PX4 与 ROS 2 之间的 Micro XRCE-DDS 通信链路。
- 将 RealSense T265 视觉定位数据转换并发布到 PX4 DDS 话题。
- 提供 C++ Offboard 底层主控，支持起飞、悬停、Offboard、降落等状态管理。
- 统一维护实机流程启动文件，把 DDS、视觉、串口、图像识别和 Offboard 控制串起来。
- 整理 ROS 端与 STM32 端的串口通信协议和实现。
- 为后续 PX4 SITL、Gazebo 和多机仿真流程保留扩展入口。

## 核心仓库

| 仓库 | 说明 |
| --- | --- |
| [BoomBoomFly](https://github.com/BoomBoomFly/BoomBoomFly) | 主工程入口，维护安装脚本、整体目录说明、仿真目录和快速启动文档。 |
| [px4_bringup](https://github.com/BoomBoomFly/px4_bringup) | PX4 无人机总启动功能包，统一启动 DDS Agent、T265、`vision_to_dds`、串口/图像识别流程和 Offboard 主控。 |
| [offboard_cpp](https://github.com/BoomBoomFly/offboard_cpp) | PX4 Offboard 模式 C++ 底层控制包，维护控制话题、状态机、起降流程和多机 Offboard 示例。 |
| [vision_to_dds](https://github.com/BoomBoomFly/vision_to_dds) | 视觉定位到 DDS 的转换节点，用于向 PX4 发布视觉里程计数据。 |
| [serial_driver_ros](https://github.com/BoomBoomFly/serial_driver_ros) | ROS 端上下位机串口通信驱动，包含通信帧协议、串口配置和示例节点。 |
| [communication](https://github.com/BoomBoomFly/communication) | 通信相关代码集合，目前主要包含 STM32F103C8T6 端 UART 收发和数据解析代码。 |
| [serial_driver_ros1](https://github.com/BoomBoomFly/serial_driver_ros1) | ROS 1 Noetic 串口通信实现。 |

## 技术栈

| 方向 | 内容 |
| --- | --- |
| 系统环境 | Ubuntu 20.04, ROS 2 Foxy |
| 飞控链路 | PX4, `px4_msgs`, Micro XRCE-DDS Agent |
| 视觉定位 | RealSense T265, `librealsense v2.53.1`, `realsense-ros 4.0.4` |
| 控制 | C++, PX4 Offboard, 有限状态机, 多机控制示例 |
| 通信 | ROS 2 serial, STM32 HAL UART, 自定义帧协议 |
| 启动管理 | ROS 2 launch, `px4_bringup` |
| 仿真规划 | PX4 SITL, Gazebo, RealSense Gazebo plugin |

T265 已不再被 Intel RealSense SDK 2.0 v2.54.1 及后续版本支持，因此 BoomBoomFly 默认固定使用 `librealsense v2.53.1` 和 `realsense-ros 4.0.4`。

## 快速开始

从主工程开始：

```bash
git clone https://github.com/BoomBoomFly/BoomBoomFly.git
cd BoomBoomFly
chmod +x Scripts/installation/uav_px4_dds_install.sh
./Scripts/installation/uav_px4_dds_install.sh
```

构建 ROS 2 工作区：

```bash
source /opt/ros/foxy/setup.bash
colcon build --symlink-install
source install/setup.bash
```

常用启动入口：

```bash
# 完整流程：PX4 / 视觉 / 串口 / 检测 / Offboard 主控
ros2 launch px4_bringup start_all_2025TI.launch.py

# PX4 DDS 基础飞行链路
ros2 launch px4_bringup px4_fly.launch.py

# Offboard 控制
ros2 launch offboard_cpp offboard_control.launch.py

# 多机 Offboard 示例
ros2 launch offboard_cpp offboard_swarm_control.launch.py
```

## 实机前检查

实际起飞前，请确认飞控、遥控器、DDS Agent、视觉里程计、电池和串口链路状态正常。

```bash
ros2 topic list
ros2 topic list | grep fmu
ros2 pkg list | grep px4_bringup
ros2 pkg list | grep offboard_cpp
```

如果没有 `/fmu/*` 话题，优先检查 PX4 端 `uxrce_dds_client`、串口设备、波特率、UDP 端口、串口权限以及当前终端是否 source 了正确的 ROS 2 工作区。

## 当前状态

BoomBoomFly 目前以无人机 PX4 DDS 实机链路为主线。安装脚本、`px4_bringup`、`offboard_cpp`、`vision_to_dds`、ROS 端串口和 STM32 端通信代码已经形成基础闭环；小车安装脚本、无人机仿真脚本、Gazebo 仿真目录和部分视觉检测扩展仍在持续补充中。

## 文档入口

- [BoomBoomFly 主工程 README](https://github.com/BoomBoomFly/BoomBoomFly)
- [Scripts 使用说明](https://github.com/BoomBoomFly/BoomBoomFly/tree/master/Scripts)
- [PX4 ROS 2 User Guide](https://docs.px4.io/main/en/ros2/)
- [Micro XRCE-DDS Agent](https://github.com/eProsima/Micro-XRCE-DDS-Agent)
- [Intel RealSense ROS](https://github.com/IntelRealSense/realsense-ros)
