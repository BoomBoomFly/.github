# BoomBoomFly

BoomBoomFly 是面向室内无人机的研究原型与工程验证项目，围绕 PX4、ROS 2、
Micro XRCE-DDS、Offboard 控制、视觉/目标感知和陆空协同任务构建可复现的多仓库工作区。

当前目标部署基线为 **Jetson Orin Nano、Ubuntu 22.04 和 ROS 2 Humble**。

## 我们在做什么

- 用 vcstool 精确提交清单恢复并验证 PX4、DDS Agent 和 ROS 2 工作区。
- 建立单一写入方的 PX4 Offboard 飞行网关及清晰的人工接管、failsafe 边界。
- 隔离视觉里程计、目标感知、任务状态机、通信和嵌入式执行职责。
- 通过 PX4 SITL 逐步验证起飞、悬停、返航、取消、降落和 DDS 重连路径。

## 主要入口

| 公开仓库 | 说明 |
| --- | --- |
| [`BoomBoomFly`](https://github.com/BoomBoomFly/BoomBoomFly) | 顶层编排、精确版本清单、工程脚本、仿真和系统文档。 |
| [`offboard_cpp`](https://github.com/BoomBoomFly/offboard_cpp) | 通用 PX4 Offboard 飞行网关。 |
| [`px4_vision_bridge`](https://github.com/BoomBoomFly/px4_vision_bridge) | 外部里程计与 PX4 EKF2 之间的视觉桥。 |
| [`px4_bringup`](https://github.com/BoomBoomFly/px4_bringup) | DDS Agent、飞行网关和视觉桥的启动入口。 |
| [`communication`](https://github.com/BoomBoomFly/communication) | 地面站、无人机和小车之间的版本化通信语义。 |

完整工作区还包括内部仓库 `common`、`TI`、`perception` 和 `embedded_systems`。它们的精确提交
由顶层 vcstool 清单管理，仅对获授权的组织成员可见。

## 开始使用

```bash
git clone https://github.com/BoomBoomFly/BoomBoomFly.git
cd BoomBoomFly
./Scripts/workspace/pull_repos.sh
./Scripts/workspace/verify_repos.py
./Scripts/workspace/verify_architecture.py
./Scripts/workspace/build.sh
```

ROS 2 Humble 的 `gz_x500` 基线已有完整 Offboard SITL 闭环证据，但验证结果不会自动外推到
Jetson、实机、新任务链或其他 ROS 2 发行版。D435i 也不原生产生位姿；完成标定、VIO 接入和
PX4 EKF2 验证之前，不得将其作为自主起降或悬停的定位来源。

详细安装、架构、验证证据与实机门禁见
[`BoomBoomFly` 主工程](https://github.com/BoomBoomFly/BoomBoomFly)。
