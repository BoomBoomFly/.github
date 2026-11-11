#JMU AIR LAB

BoomBoomFly 是面向无人系统的研发组织，当前围绕无人机（UAV）、地面车辆（UGV）、群体系统、嵌入式设备和配套知识库持续建设。组织内的仓库既包含可直接使用的公开项目，也包含用于内部协同的工作区和功能模块。

> 仓库状态和可见性会随研发阶段变化；以下内容按当前组织仓库布局整理。

## 当前研发方向

- **无人机平台**：飞控接入、ROS 2 工作区、任务编排、通信、控制与感知。
- **地面车辆**：UGV 工作区、导航、任务、底层 FPGA/嵌入式网关和启动编排。
- **群体与系统集成**：多机协同、跨平台通信、公共接口和系统级验证。
- **嵌入式与工具链**：驱动、固件、串口通信、硬件执行和工程工具。
- **知识与展示**：知识库、组织网站以及可复用的上游项目研究。

## 仓库导航

### 无人机（UAV）

| 仓库 | 可见性 | 说明 |
| --- | --- | --- |
| [`BoomBoomFly`](https://github.com/BoomBoomFly/BoomBoomFly) | 公开 | 无人机主工作区、工程脚本、版本清单和系统文档。 |
| `uav_ws` | 内部 | 无人机 ROS 2 工作区与集成入口。 |
| `uav_bringup` | 内部 | 无人机系统启动与运行编排。 |
| `uav_control` | 内部 | 无人机控制能力与飞控接口。 |
| `uav_bridge` | 内部 | 无人机与外部系统之间的桥接能力。 |
| `uav_mission` | 内部 | 无人机任务逻辑与任务级执行。 |
| `perception` | 内部 | 目标感知与任务级观测处理。 |

### 地面车辆（UGV）

| 仓库 | 可见性 | 说明 |
| --- | --- | --- |
| `ugv_ws` | 内部 | 地面车辆 ROS 2 工作区。 |
| `car_bringup` | 内部 | 车辆系统启动与集成编排。 |
| `car_navigation` | 内部 | 车辆导航能力。 |
| `car_mission` | 内部 | 车辆任务逻辑。 |
| `fpga_gateway` | 内部 | FPGA/底层设备网关。 |

### 群体、公共组件与嵌入式

| 仓库 | 可见性 | 说明 |
| --- | --- | --- |
| `swarm_ws` | 内部 | 群体系统与多机协同工作区。 |
| `common` | 内部 | 跨项目公共库、接口和基础组件。 |
| `TI` | 内部 | 面向项目任务与集成验证的工程代码。 |
| `embedded_systems` | 内部 | 嵌入式驱动、固件和硬件执行相关代码。 |
| `BoomBoomFly_knowledgebase` | 内部 | 研发知识、规范和经验沉淀。 |



## 访问与协作

内部仓库只对获授权的组织成员可见。完整工作区通常由多个仓库组成，恢复或构建内部项目前需要相应的 GitHub 访问权限；请优先阅读目标工作区仓库中的 README、构建说明和版本清单。

组织级公开入口：[`BoomBoomFly.github.io`](https://boomboomfly.github.io/)。
profile/README.md：

# BoomBoomFly

BoomBoomFly 是面向无人系统的研发组织，当前围绕无人机（UAV）、地面车辆（UGV）、群体系统、嵌入式设备和研发基础设施持续建设。

## 我们在做什么

- 构建无人机 ROS 2 工作区、飞控接入、任务控制与感知链路。
- 构建地面车辆导航、任务、启动编排和底层设备网关。
- 探索群体系统、跨平台通信和公共软件组件。
- 沉淀嵌入式驱动、工程工具、知识库和系统集成经验。

## 主要入口

| 入口 | 说明 |
| --- | --- |
| [`BoomBoomFly`](https://github.com/BoomBoomFly/BoomBoomFly) | 无人机主工作区、工程脚本、版本清单和系统文档。 |
| [`BoomBoomFly.github.io`](https://github.com/BoomBoomFly/BoomBoomFly.github.io) | 组织网站与公开信息入口。 |
| `uav_ws`、`uav_bringup`、`uav_control`、`uav_bridge`、`uav_mission` | 无人机工作区及核心功能模块。 |
| `ugv_ws`、`car_bringup`、`car_navigation`、`car_mission`、`fpga_gateway` | 地面车辆工作区及功能模块。 |
| `swarm_ws`、`common`、`TI`、`embedded_systems` | 群体系统、公共组件、任务集成与嵌入式能力。 |
| [`xtark`](https://github.com/BoomBoomFly/xtark) | 公开的机器人/嵌入式相关项目。 |
| [`open_vins`](https://github.com/BoomBoomFly/open_vins) | 基于上游项目的公开 fork。 |

完整工作区还包括内部仓库 `perception`、`BoomBoomFly_knowledgebase` 等。内部仓库仅对获授权的组织成员可见，具体构建方式和版本要求以各仓库文档为准。

欢迎从公开仓库和组织网站开始了解 BoomBoomFly。
