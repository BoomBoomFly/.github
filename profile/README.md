# JMU AIR LAB

我们是集美大学无人机科创实验室，研发无人机、地面机器人和多机器人协作系统，关注飞行控制、视觉惯性定位与机器人导航。

[实验室网站](https://boomboomfly.github.io/) · [工程知识库](https://boomboomfly.github.io/knowledge/) · [全部公开仓库](https://github.com/orgs/BoomBoomFly/repositories?type=public)

## 主要项目

| 项目 | 从这里了解 |
| --- | --- |
| [BoomBoomFly](https://github.com/BoomBoomFly/BoomBoomFly) | 实验室主开发仓库（Monorepo），统一维护 ROS 2 工作区（无人机、小车、六足与协同）、FPGA/MCU 硬件工程与外部依赖清单。 |
| [uav_ws](https://github.com/BoomBoomFly/uav_ws) | ROS 2 无人机子系统独立工作区镜像，包含 PX4 DDS/MAVROS 控制、OpenVINS 桥接、任务状态机与 SITL 仿真。 |
| [ugv_ws](https://github.com/BoomBoomFly/ugv_ws) | ROS 2 地面机器人子系统独立工作区镜像，包含 BX72 小车、ACG720 六足接入、FPGA 网关、SLAM/Nav2 与视觉跟踪。 |
| [swarm_ws](https://github.com/BoomBoomFly/swarm_ws) | 异构多机器人协同子系统独立工作区镜像，包含 AprilGrid 跨机对齐、地图融合与协同调度。 |
| [FPGA](https://github.com/BoomBoomFly/FPGA) | 高云 FPGA 板级工程（BX72 138K、ACG720 60K）及去板卡特化的 RTL/裸机算法参考库。 |

日常研发以主仓库 [BoomBoomFly](https://github.com/BoomBoomFly/BoomBoomFly) 为主线，构建与多边缘平台部署方式见主仓库 README。

## 组件与参考资料

- PX4 外部控制网关，基于 ROS 2 与 DDS。[offboard_cpp](https://github.com/BoomBoomFly/offboard_cpp)
- 基于相机与 IMU 的视觉惯性定位，上游项目的派生仓库。[OpenVINS](https://github.com/BoomBoomFly/open_vins)
- ROS 1 移动机器人和机械臂工程资料。[XTARK](https://github.com/BoomBoomFly/xtark)
- 机器人上位机与下位机的串口通信驱动。[serial_driver_ros](https://github.com/BoomBoomFly/serial_driver_ros)
