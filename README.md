# STM32 Robot Arm Controller (Auto Vision + Manual Bluetooth)

[![license](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![platform](https://img.shields.io/badge/platform-STM32F4-blue.svg)](#硬件与接线)
[![language](https://img.shields.io/badge/language-C-orange.svg)](#)

一个基于 **STM32F4 + HAL** 的机械臂控制工程，支持两种控制模式：

- **自动模式（Auto / Vision）**：通过 **USART1** 接收视觉/上位机的 **7字节固定帧**（`0xAA ... 0xFF`），实时控制底座/关节/夹爪。
- **手动模式（Manual / Bluetooth）**：通过 **USART2** 接收蓝牙遥控的 **单字节命令**，实现按住移动、松手停止等交互。

> 适合：课程设计/毕业设计/机器人控制入门/协议解析与中断调度实践。

---

## 主要特性

- 双串口输入：视觉帧（定长帧同步） + 蓝牙命令（单字节）
- TIM6 1ms 节拍器：  
  - 360°连续旋转舵机“计时转角”（开环）
  - 180°/270°舵机 20ms 平滑推进（限速 + 死区 + 量化抑抖）
- 自动流程可被手动抢占：任何手动命令会触发 **manual override**，自动演示可立即中止
- 教学友好：代码注释按“作用/原理/逻辑/并发”写法组织

---

## 目录结构（建议）

```text
.
├── Core/
│   ├── Src/                # main.c 等
│   └── Inc/
├── Drivers/                # HAL/LL
├── App/
│   ├── UART.c/.h           # 串口协议解析（视觉帧/蓝牙命令）
│   ├── Timer.c/.h          # TIM6节拍器 + 平滑推进
│   ├── Servo.c/.h          # PWM输出层
│   ├── movement.c/.h       # 动作编排层（演示脚本、语义接口）
│   └── ...
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
└── SECURITY.md
