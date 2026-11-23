# RM2025 超级电容与无线充电系统

香港科技大学 RoboMaster ENTERPRIZE 战队的 RM2025 开源工程。本仓库由 FIyChan 从上游 fork，补充中文阅读索引。硬件、固件和技术报告由原作者发布。

## 项目简介

系统面向 RoboMaster 机器人底盘。超级电容组通过双向 Buck-Boost 与底盘直流母线交换能量，在底盘功率需求上升时供能，也可在制动时回收能量。功控板使用 STM32G474，由软件外环更新电流目标，并由 DAC、比较器和 HRTIM 配合完成逐开关周期的峰值电流控制。车载无线充电接收端与超级电容控制共用一颗 MCU。

## 仓库目录

| 入口 | 内容 |
| --- | --- |
| [功控板控制上板](PowerControl_PCM_V0.1_Control/) | MCU、采样与控制电路 |
| [功控板功率下板](PowerControl_PCM_V0.1_Power/) | 双向 Buck-Boost 功率级及无线接收侧相关功率电路 |
| [无线充电接收线圈](PowerControlBoard_RXCoil_V0.1/) | 接收线圈板与生产资料 |
| [无线充电发射端硬件](WirelessCharger_V0.1/) | 发射端 KiCad 工程 |
| [功控板赛场版固件](PowerControlBoard_Software_FieldVersion/) | STM32G474 源码、CubeMX 配置、Makefile 与 CAN 协议说明 |
| [技术报告](技术报告/) | 原理图、控制框图、设计说明与测试记录 |
| [无线充电发射端固件](https://github.com/hkustenterprize/RM2025-Wireless-Charging-Public) | 上游单独发布的发射端代码 |

## 从哪里开始读

1. **先看系统拓扑。** 技术报告第 2 节介绍超级电容与无线充电的硬件架构。先找出电容侧、母线侧和双向功率变换器，再顺着充电、放电两种工况追踪能量流向。
2. **再看电流闭环。** 阅读报告第 3.3 节，结合固件的 [PowerManager.cpp](PowerControlBoard_Software_FieldVersion/Core/Src/PowerManager.cpp) 查看外环，再对照 [hrtim.c](PowerControlBoard_Software_FieldVersion/Core/Src/hrtim.c)、[comp.c](PowerControlBoard_Software_FieldVersion/Core/Src/comp.c) 和 [dac.c](PowerControlBoard_Software_FieldVersion/Core/Src/dac.c) 查看硬件内环配置。
3. **最后看整车接口。** 阅读[固件说明](PowerControlBoard_Software_FieldVersion/README.md)中的 CAN 帧格式，追踪底盘控制器发来的功率限制与功控板反馈的电容能量、底盘功率和故障状态。

## 工程打开方式

- **硬件**使用 KiCad 8.0 打开对应目录内的 `.kicad_pro`，从原理图查看电路，从 PCB 编辑器查看布局与走线。
- **固件**使用 STM32CubeMX 打开固件目录内的 `.ioc` 查看外设配置。工程的构建入口为同目录下的 `Makefile`，使用 GNU Arm 工具链。本次整理只核对了源码和工程入口，尚未进行固件构建与实物测试。

## 来源与许可证

- 原工程来自[香港科技大学 ENTERPRIZE 战队](https://github.com/hkustenterprize/RM2025-PowerControlBoard-WirelessCharging)。作者名单、测试数据及已知问题见[上游 README](https://github.com/hkustenterprize/RM2025-PowerControlBoard-WirelessCharging/blob/main/README.md)与 [RoboMaster 开源帖](https://bbs.robomaster.com/article/761385)。
- 仓库保留上游 [GPL-3.0 许可证](LICENSE)。STM32 HAL、CMSIS 等第三方组件请查阅各自目录中的许可文本。论坛原文的许可见原帖。
