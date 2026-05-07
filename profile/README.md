# BoomBoomFly

BoomBoomFly 是围绕 PX4、ROS 2 和 DDS 通信链路整理的无人机工程集合，聚焦实机调试、视觉定位、Offboard 控制和上下位机通信。

## 方向

- PX4 与 ROS 2 Foxy 的 Micro XRCE-DDS 通信。
- RealSense T265 视觉里程计接入。
- C++ Offboard 底层控制与比赛启动流程。
- ROS 端与 STM32 端串口通信。

## 入口

| 仓库 | 说明 |
| --- | --- |
| [BoomBoomFly](https://github.com/BoomBoomFly/BoomBoomFly) | 主工程、安装脚本和整体文档。 |
| [px4_bringup](https://github.com/BoomBoomFly/px4_bringup) | PX4 / DDS / T265 / Offboard 启动集合。 |
| [offboard_cpp](https://github.com/BoomBoomFly/offboard_cpp) | PX4 Offboard C++ 底层控制。 |
| [serial_driver_ros](https://github.com/BoomBoomFly/serial_driver_ros) | ROS 上下位机串口通信。 |
| [communication](https://github.com/BoomBoomFly/communication) | STM32 端通信代码。 |

详细安装和启动流程见 [BoomBoomFly 主工程 README](https://github.com/BoomBoomFly/BoomBoomFly)。
