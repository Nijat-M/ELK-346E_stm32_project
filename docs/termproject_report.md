# ELK-346E Term Project Report
# 基于STM32的环境控制系统 - 期末项目报告

## 🇬🇧 English Version

### Project Information

**Project Title:** STM32-based Environment Control System  
**Course:** ELK-346E  
**Date:** February 2026  

---

### 1. Introduction

This project implements an automated environment monitoring and control system using the STM32F103C8T6 microcontroller. The system reads environmental data (temperature and humidity) and automatically controls actuators (fan and humidifier) to maintain comfortable conditions.

---

### 2. System Architecture

#### 2.1 Hardware Components

| Component | Model/Type | Connection | Purpose |
|-----------|------------|------------|---------|
| Microcontroller | STM32F103C8T6 (Blue Pill) | - | Main processing unit |
| Temperature/Humidity Sensor | DHT11 | PB9 | Environmental data acquisition |
| Display | SSD1306 OLED (I2C) | PB6 (SCL), PB7 (SDA) | Data visualization |
| Fan | 5V DC Fan | PB4 | Temperature control |
| Humidifier | Steam Generator | PB10 | Humidity control |

#### 2.2 Software Architecture

The system is built using:
- **HAL (Hardware Abstraction Layer):** For STM32 peripheral control
- **Custom DHT11 Driver:** For sensor data acquisition
- **SSD1306 Driver:** For OLED display control (from [afiskon/stm32-ssd1306](https://github.com/afiskon/stm32-ssd1306))

---

### 3. Implementation Details

#### 3.1 Sensor Data Acquisition

The DHT11 sensor provides:
- Temperature readings (0-50°C, ±2°C accuracy)
- Humidity readings (20-90% RH, ±5% accuracy)
- Data includes checksum validation for reliability

#### 3.2 Control Logic

**Fan Control:**
```
IF temperature > 26°C THEN
    Fan ON (PB4 = HIGH)
ELSE
    Fan OFF (PB4 = LOW)
END IF
```

**Humidifier Control:**
```
IF humidity < 65% THEN
    Humidifier ON (PB10 = LOW)
ELSE
    Humidifier OFF (PB10 = HIGH)
END IF
```

#### 3.3 Display Interface

The OLED screen displays:
- Current temperature (°C)
- Current humidity (%)
- Fan status (ON/OFF)

---

### 4. Testing and Results

#### 4.1 Test Scenarios

1. **Temperature Control Test:**
   - Tested temperature threshold at 26°C
   - Fan activation verified when temperature exceeds threshold
   - Fan deactivation verified when temperature drops

2. **Humidity Control Test:**
   - Tested humidity threshold at 65%
   - Humidifier activation verified when humidity drops below threshold
   - Humidifier deactivation verified when humidity increases

3. **Display Test:**
   - Verified accurate display of sensor readings
   - Confirmed real-time updates every second

4. **Data Integrity Test:**
   - Tested checksum validation
   - Verified system uses last valid readings on checksum failure

#### 4.2 Results

- System successfully maintains environmental conditions within target ranges
- Response time: < 1 second for threshold detection
- Display update rate: 1 Hz
- Sensor reading accuracy: Within DHT11 specifications

---

### 5. Challenges and Solutions

#### 5.1 Challenge: DHT11 Timing Requirements

**Problem:** DHT11 requires precise timing for communication protocol

**Solution:** Implemented delay functions with microsecond precision using HAL_Delay and custom timing loops

#### 5.2 Challenge: Display Refresh

**Problem:** OLED display flicker during rapid updates

**Solution:** Implemented clear and redraw strategy with appropriate delays

---

### 6. Conclusions

The project successfully demonstrates:
- Integration of multiple peripheral devices with STM32
- Real-time environmental monitoring and control
- Reliable sensor data acquisition with error checking
- User-friendly display interface

The system provides a practical solution for maintaining comfortable environmental conditions automatically.

---

### 7. Future Improvements

- Add Wi-Fi connectivity for remote monitoring
- Implement data logging to SD card
- Add adjustable threshold settings via buttons or mobile app
- Include additional sensors (CO2, air quality)
- Add PID control for smoother temperature/humidity regulation

---

## 🇨🇳 中文版本

### 项目信息

**项目名称：** 基于STM32的环境控制系统  
**课程：** ELK-346E  
**日期：** 2026年2月  

---

### 1. 引言

本项目使用 STM32F103C8T6 微控制器实现了一个自动化的环境监控系统。系统读取环境数据（温度和湿度），并自动控制执行器（风扇和加湿器）以保持舒适的环境条件。

---

### 2. 系统架构

#### 2.1 硬件组成

| 组件 | 型号/类型 | 连接方式 | 用途 |
|------|-----------|----------|------|
| 微控制器 | STM32F103C8T6 (蓝色核心板) | - | 主处理单元 |
| 温湿度传感器 | DHT11 | PB9 | 环境数据采集 |
| 显示屏 | SSD1306 OLED (I2C) | PB6 (SCL), PB7 (SDA) | 数据可视化 |
| 风扇 | 5V 直流风扇 | PB4 | 温度控制 |
| 加湿器 | 蒸汽发生器 | PB10 | 湿度控制 |

#### 2.2 软件架构

系统使用：
- **HAL（硬件抽象层）：** 用于 STM32 外设控制
- **自定义 DHT11 驱动：** 用于传感器数据采集
- **SSD1306 驱动：** 用于 OLED 显示控制（来自 [afiskon/stm32-ssd1306](https://github.com/afiskon/stm32-ssd1306)）

---

### 3. 实现细节

#### 3.1 传感器数据采集

DHT11 传感器提供：
- 温度读数（0-50°C，±2°C 精度）
- 湿度读数（20-90% RH，±5% 精度）
- 数据包含校验和验证以确保可靠性

#### 3.2 控制逻辑

**风扇控制：**
```
如果 温度 > 26°C 则
    风扇开启（PB4 = 高电平）
否则
    风扇关闭（PB4 = 低电平）
结束
```

**加湿器控制：**
```
如果 湿度 < 65% 则
    加湿器开启（PB10 = 低电平）
否则
    加湿器关闭（PB10 = 高电平）
结束
```

#### 3.3 显示界面

OLED 屏幕显示：
- 当前温度（°C）
- 当前湿度（%）
- 风扇状态（开/关）

---

### 4. 测试与结果

#### 4.1 测试场景

1. **温度控制测试：**
   - 测试 26°C 温度阈值
   - 验证温度超过阈值时风扇启动
   - 验证温度下降时风扇关闭

2. **湿度控制测试：**
   - 测试 65% 湿度阈值
   - 验证湿度低于阈值时加湿器启动
   - 验证湿度上升时加湿器关闭

3. **显示测试：**
   - 验证传感器读数显示准确
   - 确认每秒实时更新

4. **数据完整性测试：**
   - 测试校验和验证
   - 验证校验失败时系统使用上次有效读数

#### 4.2 结果

- 系统成功将环境条件维持在目标范围内
- 响应时间：阈值检测 < 1秒
- 显示更新速率：1 Hz
- 传感器读数精度：符合 DHT11 规格

---

### 5. 挑战与解决方案

#### 5.1 挑战：DHT11 时序要求

**问题：** DHT11 通信协议需要精确的时序

**解决方案：** 使用 HAL_Delay 和自定义时序循环实现微秒级精度的延迟函数

#### 5.2 挑战：显示刷新

**问题：** 快速更新时 OLED 显示闪烁

**解决方案：** 实现清除和重绘策略，并使用适当的延迟

---

### 6. 结论

项目成功演示了：
- STM32 与多个外设的集成
- 实时环境监控与控制
- 带错误检查的可靠传感器数据采集
- 用户友好的显示界面

该系统为自动维持舒适的环境条件提供了实用的解决方案。

---

### 7. 未来改进

- 添加 Wi-Fi 连接以实现远程监控
- 实现数据记录到 SD 卡
- 通过按钮或移动应用添加可调阈值设置
- 包含额外的传感器（CO2、空气质量）
- 添加 PID 控制以实现更平滑的温湿度调节

---

## Appendices

### Appendix A: Circuit Diagram
*[Add circuit diagram here]*

### Appendix B: Source Code
Source code is available in the repository at:
- Main application: `Core/Src/main.c`
- DHT11 driver: `Core/Inc/dht11.h` and related source files
- Display driver: `Drivers/` directory

### Appendix C: References

1. STM32F103C8T6 Reference Manual
2. DHT11 Temperature and Humidity Sensor Datasheet
3. SSD1306 OLED Display Datasheet
4. [afiskon/stm32-ssd1306 GitHub Repository](https://github.com/afiskon/stm32-ssd1306)

---

**Document Version:** 1.0  
**Last Updated:** February 2026
