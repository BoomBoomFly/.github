# BoomBoomFly

BoomBoomFly 是面向室内无人机的研究原型与工程验证项目，围绕 PX4、ROS 2、
Micro XRCE-DDS、Offboard 控制、视觉/目标感知和陆空协同任务构建可复现的多仓库工作区。

当前目标部署基线为 **Jetson Orin Nano、Ubuntu 22.04 和 ROS 2 Humble**。顶层
[`BoomBoomFly`](https://github.com/BoomBoomFly/BoomBoomFly) 仓库只负责编排清单、工程脚本、
版本、构建、仿真与系统文档；各功能仓库保留独立 Git 历史，并通过 vcstool 清单锁定精确提交。

## 项目重点

- 用精确提交清单恢复 PX4、DDS Agent、ROS 2 包及可选感知依赖，建立可验证的工作区基线。
- 通过 `offboard_cpp` 提供唯一 PX4 Offboard 飞行网关，明确控制权、人工接管和 failsafe 边界。
- 通过 `px4_vision_bridge` 隔离视觉里程计接入与飞行控制，完整处理坐标系、时间戳和有效性。
- 通过 `ti`、`perception`、`communication` 和 `embedded_systems` 组织任务、观测、通信及硬件执行。
- 持续验证 PX4 SITL、DDS 重连、任务取消和安全降落；实机能力只在完成对应门禁后声明。

## 工作区与职责

```text
BoomBoomFly/
├── manifests/                  # vcstool 精确提交清单
├── Scripts/                    # 拉取、构建、验证和 SITL 入口
├── docs/                       # 架构、交接、演进路线和实机门禁
└── px4/
    ├── upstream/               # PX4-Autopilot、Micro-XRCE-DDS-Agent
    └── px4_ws/src/
        ├── external/           # px4_msgs 与可选第三方依赖
        └── boomboom/           # 各自独立的自研 ROS 2 仓库
```

| 仓库 | 可见性 | 职责 |
| --- | --- | --- |
| [`BoomBoomFly`](https://github.com/BoomBoomFly/BoomBoomFly) | 公开 | 顶层编排、精确版本清单、工程脚本、仿真和系统文档。 |
| `common` | 内部 | 公共 ROS 2 `msg/srv/action`、状态码和错误码。 |
| [`offboard_cpp`](https://github.com/BoomBoomFly/offboard_cpp) | 公开 | 通用 PX4 飞行网关，独占生产环境的 Offboard 控制输入。 |
| [`px4_vision_bridge`](https://github.com/BoomBoomFly/px4_vision_bridge) | 公开 | 将外部里程计或兼容视觉输入送入 PX4 EKF2，不控制飞行。 |
| [`px4_bringup`](https://github.com/BoomBoomFly/px4_bringup) | 公开 | 启动 DDS Agent、飞行网关和视觉桥。 |
| [`communication`](https://github.com/BoomBoomFly/communication) | 公开 | 定义地面站、无人机和小车间的版本化通信语义。 |
| `TI` | 内部 | 任务生命周期、导航适配、H/D 赛题和任务级入口。 |
| `perception` | 内部 | 产生带时间与有效性语义的任务级目标观测。 |
| `embedded_systems` | 内部 | 嵌入式驱动与固件、小车控制及硬件命令执行。 |

内部仓库只对获授权的组织成员可见。顶层 vcstool 清单同时记录公开和内部仓库；恢复完整工作区
需要对应的 GitHub 访问权限。

## 技术基线

| 方向 | 当前基线 |
| --- | --- |
| 目标平台 | Jetson Orin Nano、Ubuntu 22.04、ROS 2 Humble |
| 飞控与通信 | PX4、`px4_msgs`、Micro XRCE-DDS Agent |
| 控制 | C++、ROS 2 Action、PX4 Offboard、可单元测试的任务 FSM |
| 仿真 | PX4 SITL；Ubuntu 22.04 默认 `gz_x500` |
| 视觉与感知 | RealSense D435i、可选 `librealsense` / `realsense-ros`、独立 VIO/感知链路 |
| 复现方式 | vcstool 清单锁定 40 位提交，脚本检查远端、HEAD、脏工作树和架构约束 |

BehaviorTree.CPP 和第三方规划器目前不是任务核心依赖。只有当任务并行、重试和条件分支显著增长时，
才评估行为树；规划器只能通过导航适配层接入，不能绕过飞行网关直接写 PX4 控制话题。

## 快速开始

需要先安装 ROS 2 Humble、vcstool 以及各功能包声明的系统依赖。

```bash
git clone https://github.com/BoomBoomFly/BoomBoomFly.git
cd BoomBoomFly

# 恢复清单中的核心仓库
./Scripts/workspace/pull_repos.sh

# 检查精确版本、远端、工作树和架构约束
./Scripts/workspace/verify_repos.py
./Scripts/workspace/verify_architecture.py

# 构建全部自研 ROS 2 包，或指定一个包及其依赖
./Scripts/workspace/build.sh
./Scripts/workspace/build.sh offboard_cpp

# 构建并运行 PX4 SITL
bash Scripts/simulation/build_px4_sitl.sh
bash Scripts/simulation/run_px4_sitl.sh
```

如需从源码恢复可选 RealSense 依赖：

```bash
./Scripts/workspace/pull_repos.sh --with-perception-deps
./Scripts/workspace/verify_repos.py --with-perception-deps
```

PX4-Autopilot 和 Micro-XRCE-DDS-Agent 位于 `px4/upstream/`，不由 colcon 构建。

## 当前验证状态

ROS 2 Humble 的 `gz_x500` 基线已完成真实 RC 解锁沿触发的 Offboard 闭环验证，包括 1.5 m
起飞、约 60 s 悬停、返航、降落和解除解锁；任务取消后的 `CANCELLED`、降落 ACK 与新的
`landed = true` 样本也已有 SITL 证据。

这些结果不能自动外推到新 Action gateway、TI 任务链、Jetson、实机或其他 ROS 2 发行版。
受控 DDS Agent 重连后的 timesync 恢复，也不能替代自然时间跳变的根因分析。

## 飞行与感知安全边界

- D435i 提供 RGB、深度和原始 IMU，但不是 T265 的即插即用替代品，也不原生产生位姿。
- 相机—IMU 标定、视觉里程计接入和 PX4 EKF2 验证完成前，不得使用 D435i 支撑自主起降或悬停。
- 每个生产 `/fmu/in/*` 控制话题只能有一个写入方；回放必须隔离 `ROS_DOMAIN_ID` 且不得接入生产飞控。
- PX4/RC 始终拥有 ARM、DISARM、Kill、人工接管和 failsafe 的最终权限；工程脚本不授权自动解锁或飞行。
- 实机操作必须完成 Kill、遥控器、QGC、机体、桨叶、定位和链路检查，并遵循实机门禁清单。

## 文档入口

- [主工程 README](https://github.com/BoomBoomFly/BoomBoomFly)
- [工作区架构](https://github.com/BoomBoomFly/BoomBoomFly/blob/master/docs/%E5%B7%A5%E4%BD%9C%E5%8C%BA%E6%9E%B6%E6%9E%84.md)
- [当前交接与验证边界](https://github.com/BoomBoomFly/BoomBoomFly/blob/master/docs/handoff.md)
- [参考框架采用与演进路线](https://github.com/BoomBoomFly/BoomBoomFly/blob/master/docs/%E5%8F%82%E8%80%83%E6%A1%86%E6%9E%B6%E9%87%87%E7%94%A8%E4%B8%8E%E6%BC%94%E8%BF%9B%E8%B7%AF%E7%BA%BF.md)
- [第一阶段实机门禁清单](https://github.com/BoomBoomFly/BoomBoomFly/blob/master/docs/%E7%AC%AC%E4%B8%80%E9%98%B6%E6%AE%B5%E5%AE%9E%E6%9C%BA%E9%97%A8%E7%A6%81%E6%B8%85%E5%8D%95.md)
