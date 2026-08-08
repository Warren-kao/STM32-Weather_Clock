<div align="center">

# 🌤️ STM32 智能天气时钟

[![GitHub stars](https://img.shields.io/github/stars/Warren-kao/STM32-Weather_Clock?style=for-the-badge&color=yellow)](https://github.com/Warren-kao/STM32-Weather_Clock/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/Warren-kao/STM32-Weather_Clock?style=for-the-badge&color=blue)](https://github.com/Warren-kao/STM32-Weather_Clock/network)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-STM32-blue?style=for-the-badge&logo=stmicroelectronics)](https://www.st.com/)

<p align="center">
  <i>📟 我的第一个嵌入式项目</i>
</p>

---

</div>

## 📖 项目简介

这是我的第一个完整嵌入式项目，一个基于 **STM32** 的智能天气时钟。做这个项目的目的是把学习过程中零散的知识点整合起来，包括：

- GPIO 输入输出
- 定时器中断
- 串口通信（调试输出）
- I2C 通信（OLED 屏幕）
- 传感器驱动（温湿度）
- 模块化编程（.h / .c 分离）

> 🎯 **目标**：通过一个完整的项目，巩固 C 语言和 STM32 的基础知识，养成良好编程习惯。

---

## 🛠️ 技术栈

| 类别 | 选型 |
|:---|:---|
| **主控芯片** | STM32F103C8T6 |
| **开发环境** | Keil MDK 5 |
| **固件库** | STM32 标准库 |
| **温湿度传感器** | DHT11 |
| **显示** | 0.96寸 OLED（I2C 接口）|
| **时钟模块** | DS1302（规划中）|
| **调试接口** | USART1（串口打印）|
| **硬件设计** | 嘉立创 EDA（规划中）|

---

## 📁 项目目录结构（规划）

```

Project/
├── EDA/                                   # 嘉立创 EDA 工程文件
│   ├── Weather_Clock_V1.0.json            # 嘉立创 EDA 工程（原理图+PCB）
│   ├── Weather_Clock_V1.0.pdf             # 原理图 PDF 导出版本
│   ├── Weather_Clock_V1.0_BOM.csv         # BOM 物料清单
│   └── Gerber/                            # PCB 生产文件
│       └── Weather_Clock_V1.0_Gerber.zip
├── Start/                                 # 启动文件 & 内核文件
│   ├── startup_stm32f10x_md.s             # 启动文件（中等容量）
│   ├── core_cm3.c/h                       # Cortex-M3 内核文件
│   ├── stm32f10x.h                        # STM32 外设寄存器定义
│   └── system_stm32f10x.c/h               # 系统时钟初始化
│
├── Library/                               # STM32 标准库外设驱动
│   ├── misc.c/h                           # 内核外设（NVIC 等）
│   ├── stm32f10x_gpio.c/h                 # GPIO
│   ├── stm32f10x_usart.c/h                # 串口
│   ├── stm32f10x_i2c.c/h                  # I2C
│   ├── stm32f10x_tim.c/h                  # 定时器
│   ├── stm32f10x_adc.c/h                  # ADC
│   ├── stm32f10x_dma.c/h                  # DMA
│   └── ...                                # 其他标准库外设
│
├── System/                                # 系统级基础模块（内部外设封装）
│   ├── Delay.c/h                          # 延时函数（软件延时）
│   ├── Timer.c/h                          # 定时器配置与中断
│   └── ...                                # 其他(如果有)
│
├── Hardware/                              # 硬件驱动（自己写的）
│   ├── LED.c/h                            # LED 控制
│   ├── Key.c/h                            # 按键检测（消抖）
│   ├── OLED.c/h                           # OLED 显示驱动（I2C）
│   ├── OLED_Font.h                        # OLED 字体库
│   ├── Serial.c/h                         # 串口通信（printf 重定向）
│   ├── DHT11.c/h                          # DHT11 温湿度传感器
│   └── ...                                # 其他驱动(如果有)
│
├── User/                                  # 应用层
│   ├── main.c                             # 主程序入口
│   ├── stm32f10x_conf.h                   # 外设配置文件
│   └── stm32f10x_it.c/h                   # 中断服务函数
│
└── Project.uvprojx                        # Keil 工程文件

```

### 分层说明

| 层级 | 目录 | 职责 |
|:---|:---|:---|
| **硬件设计** | `EDA/` | 嘉立创 EDA 工程文件、原理图、PCB、BOM、Gerber 生产文件 |
| **启动 & 内核** | `Start/` | 启动文件、Cortex-M3 内核、寄存器定义、系统时钟初始化 |
| **标准库** | `Library/` | STM32 官方固件库，提供外设寄存器操作 API |
| **系统层** | `System/` | 芯片内部资源封装：延时、定时器、时钟等 |
| **硬件层** | `Hardware/` | 板载外设驱动：LED、按键、OLED、传感器等 |
| **应用层** | `User/` | 业务逻辑：数据采集、显示刷新、任务调度 |

> 💡 **依赖关系**：应用层 → 硬件层 → 系统层 → 标准库 → 启动/内核。底层变动不影响上层，代码清晰易维护。

---

## 🔌 硬件连接（规划）

| 外设 | 引脚 | 说明 |
|:---|:---|:---|
| OLED (I2C) | PB6 (SCL) / PB7 (SDA) | 屏幕显示 |
| DHT11 | PA1 | 温湿度数据 |
| DS1302 | PA1/PA2/PA3 | 时钟模块（备用）|
| USART1 | PA9 (TX) / PA10 (RX) | 串口调试 |
| LED | PC13 | 状态指示 |

---

## 🚀 开发计划

- [x] 搭建 Keil 工程，配置标准库
- [x] 实现 System 层：Delay / Timer
- [x] 实现 Hardware 驱动：OLED（I2C）显示测试
- [x] 实现 Hardware 驱动：DHT11 温湿度读取
- [x] 实现 User 应用层：数据采集与显示刷新
- [ ] 嘉立创 EDA 原理图设计
- [ ] PCB Layout 与打样

---

## 📄 开源协议

[MIT](LICENSE) © Warren-kao

---

<p align="center">
  <i>⭐ 这只是一个开始，我会继续加油 💪</i>
</p>
