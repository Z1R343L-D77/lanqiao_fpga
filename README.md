# LanQiao FPGA 实验项目集

本项目包含基于Xilinx Vivado工具链的FPGA基础实验,涵盖LED控制、定时器、数码管显示、按键处理、蜂鸣器、ADC采集、DS1302实时时钟等模块。

## 项目结构

```
lanqiao_fpga/
├── 001_led_blink/      # LED闪烁实验
├── 002_led_water/      # LED流水灯实验
├── 003_tim/            # 定时器分频实验
├── 004_tim_led_key/    # 定时器+LED+按键实验
├── 005_buzz/           # 蜂鸣器实验
├── 006_smg/            # 数码管显示实验
├── 007_key_led/        # 按键控制LED实验
├── 008_key_smg_cnt/    # 按键控制数码管计数实验
├── 010_adc/            # ADC模数转换实验
├── 011_ds1302/         # DS1302实时时钟实验
└── 100_sim2/           # 仿真测试实验
```

## 实验列表

### 001_led_blink - LED闪烁
- **功能**: 50MHz系统时钟分频实现0.1秒周期LED闪烁
- **核心模块**: led.v
- **技术要点**: 计数器分频、时序逻辑

### 002_led_water - LED流水灯
- **功能**: 8个LED依次点亮实现流水效果
- **核心模块**: led.v
- **技术要点**: 状态机控制、位操作

### 003_tim - 定时器分频
- **功能**: 可配置分频比的通用分频器
- **核心模块**: freq_tim.v, main.v
- **技术要点**: 参数化设计、分频算法

### 004_tim_led_key - 综合实验
- **功能**: 按键切换LED显示模式
- **核心模块**: main.v, key_proc.v, led_proc.v, time_proc.v
- **技术要点**: 模式切换、按键消抖

### 005_buzz - 蜂鸣器
- **功能**: 按键控制蜂鸣器发声
- **核心模块**: main.v
- **技术要点**: PWM控制、时序控制

### 006_smg - 数码管显示
- **功能**: 数码管动态扫描显示
- **核心模块**: smg.v
- **技术要点**: 动态扫描、段选/位选控制

### 007_key_led - 按键控制LED
- **功能**: 4个按键切换LED显示模式
- **核心模块**: top.v, key_proc.v, led_proc.v
- **技术要点**: 按键检测、模式切换

### 008_key_smg_cnt - 数码管计数
- **功能**: 按键控制数码管数值增减
- **核心模块**: main.v, key_proc.v, smg_ctrl.v
- **技术要点**: 数值处理、动态显示

### 010_adc - ADC采集
- **功能**: I2C接口ADC采集电压并显示
- **核心模块**: top.v, adc.v, i2c.v, smg_proc.v
- **技术要点**: I2C协议、ADC驱动

### 011_ds1302 - 实时时钟
- **功能**: DS1302实时时钟读写,显示时间
- **核心模块**: top.v, rtc_proc.v, ds1302_ctrl.v, ds1302_io.v
- **技术要点**: SPI协议、RTC驱动

### 100_sim2 - 仿真测试
- **功能**: 计时器功能仿真测试
- **核心模块**: main.v, test_bench.v
- **技术要点**: Testbench编写、时序仿真

## 硬件平台

- **FPGA系列**: Xilinx Spartan系列
- **开发工具**: Xilinx Vivado
- **时钟频率**: 50MHz
- **复位方式**: 高电平有效(rst)或低电平有效(rst_n)

## 文件说明

### 源文件类型
- `.v` - Verilog HDL源文件
- `.xdc` - 约束文件(XDC格式)
- `.tcl` - Vivado脚本文件

### 模块命名规范
- `*_proc` - 处理器/控制模块
- `*_ctrl` - 控制器模块
- `*_driver` - 驱动模块
- `*_proc` - 处理模块

## 使用方法

1. 使用Xilinx Vivado打开对应实验目录下的`.xpr`项目文件
2. 点击"Generate Bitstream"生成比特流
3. 通过JTAG下载到FPGA开发板
4. 观察实验现象

## 核心模块说明

### 分频器 (freq_tim/time_proc)
```verilog
module freq_tim(
    input wire clk,           // 系统时钟
    input wire rst_n,         // 复位信号
    input wire [31:0] clkbase, // 基准时钟频率
    input wire [31:0] clkdiv,  // 目标分频频率
    output reg clkout         // 输出时钟
);
```

### 按键消抖 (key_proc/key_ctrl)
- 1kHz采样时钟
- 20ms消抖延迟
- 边沿检测(按下/释放)

### 数码管驱动 (smg/seg_driver)
- 8位动态扫描
- 1kHz扫描频率
- 共阳极编码

### I2C/SPI接口
- 标准协议实现
- 状态机控制
- 可配置时钟分频

## 技术要点

1. **时序逻辑**: 所有模块使用同步时序设计
2. **参数化设计**: 分频器支持参数配置
3. **状态机**: 复杂控制使用有限状态机
4. **边沿检测**: 按键使用下降沿/上升沿检测
5. **动态扫描**: 数码管采用动态扫描显示

## 注意事项

1. 约束文件(xdc)需根据实际硬件修改引脚分配
2. 仿真测试需使用Vivado Simulator或ModelSim
3. 部分模块需要外部硬件支持(ADC、DS1302等)
4. 所有模块假设系统时钟为50MHz

## 扩展开发

- 可添加UART串口通信模块
- 可扩展PWM输出模块
- 可添加外部RAM接口
- 可实现电机控制模块

## 许可证

本项目仅用于教学和学习目的。
