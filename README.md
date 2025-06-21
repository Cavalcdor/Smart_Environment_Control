# Smart_Environment_Control  

基于多模态感知与模糊控制的智能环境调节系统  


## **目录**  
- [项目概述](#项目概述)  
- [项目架构](#项目架构)  
- [系统架构](#系统架构)  
- [功能特性](#功能特性)  
- [技术栈](#技术栈)  
- [安装与配置](#安装与配置)  
- [使用方法](#使用方法)  
- [配置参数](#配置参数)  
- [系统测试](#系统测试)  
- [技术原理](#技术原理)  
- [贡献指南](#贡献指南)  
- [许可证](#许可证)  
- [开发者](#开发者)  


## **项目概述**  
Smart_Environment_Control 是一个创新型智能家居系统，通过融合**表情识别、心率监测和环境感知**技术，实现“情绪-环境”自适应联动。系统采用Arduino MEGA2560作为主控平台，结合Python电脑端程序，通过模糊控制算法动态调节灯光色温、亮度及模拟电器状态，为用户创造个性化的舒适环境。  


## **项目架构**  
### **目录结构**  
```  
Smart_Environment_Control/  
├── LICENSE                 # 开源许可证（MIT）
├── README.md               # 项目说明文档
├── Docs/                   # 项目文档
│   ├── Design_Specs/       # 设计文档
│   ├── Test_Reports/       # 测试报告
│   └── User_Manual.md      # 用户手册
├── Hardware/               # 硬件相关文件
│   ├── Circuit_Diagrams/   # 电路原理图
│   ├── PCB_Layout/         # PCB设计文件
│   └── Components/         # 元器件清单
├── Software/               # 软件代码
│   ├── Computer/           # 电脑端（Python） 
│   │   ├── Src/            # 源代码  
│   │   │   ├── Emotion_Detection.py   # 表情识别模块  
│   │   │   ├── Heart_Rate_Processing.py # 心率处理模块  
│   │   │   ├── Bluetooth_Communication.py # 蓝牙通信模块  
│   │   │   └── Main.py         # 主程序入口  
│   │   ├── Tests/          # 测试代码  
│   │   ├── Requirements.txt    # Python依赖  
│   │   └── Config/           # 配置文件  
│   └── Arduino/            # Arduino端  
│       ├── Src/            # 源代码  
│       │   ├── Main.ino      # 主程序  
│       │   ├── Sensors/      # 传感器驱动  
│       │   ├── Actuators/    # 执行器控制  
│       │   └── Fuzzy_Logic/  # 模糊控制算法  
│       ├── Libraries/      # 自定义库  
│       └── Tests/          # 测试代码  
├── Data/                   # 数据文件  
│   ├── Training_Data/      # 训练数据集  
│   ├── Test_Data/          # 测试数据集  
│   └── Logs/               # 系统运行日志  
└── Config/                 # 全局配置文件  
    └── Fuzzy_Rules.json    # 模糊控制规则表  
```  

### **架构设计原则**  
1. **模块化分离**：硬件、软件、文档独立管理，便于分工协作  
2. **关注点分离**：电脑端负责AI处理，Arduino端负责实时控制  
3. **配置中心化**：关键参数集中在`Config/`目录，便于统一管理  
4. **可扩展性**：预留接口支持新传感器（如空气质量）或执行器（如窗帘控制）  


## **系统架构**  
### 硬件组成  
| 模块               | 功能描述                          | 关键参数                     |  
|--------------------|-----------------------------------|------------------------------|  
| Arduino MEGA2560   | 主控制器                          | 54路数字IO，16路模拟输入     |  
| MAX30100           | 心率血氧传感器                    | 反射式光学检测，采样率100Hz  |  
| WS2812 LED         | 全彩灯光控制                      | 24位色彩，5V供电             |  
| DHT11              | 温湿度传感器                      | 温度范围0-50℃，湿度20-90%RH  |  
| 光敏传感器         | 环境光照检测                      | 模拟量输出，检测范围0-1000lux|  
| 继电器模块         | 模拟电器控制                      | 3路5V触发，触点容量10A/250VAC|  

### 软件架构  
1. **电脑端（Python）**：  
   - 表情识别（MediaPipe）  
   - 心率数据处理与滤波  
   - 蓝牙通信控制  
2. **Arduino端**：  
   - 传感器数据采集  
   - 模糊控制算法实现  
   - 执行器驱动控制  


## **功能特性**  
1. **多模态感知**：  
   - 实时面部表情识别（微笑/中性/皱眉）  
   - 心率监测与异常检测  
   - 环境温湿度及光照强度采集  

2. **智能决策**：  
   - 基于模糊控制理论的环境调节策略  
   - 多参数融合决策（情绪+生理+环境）  

3. **自适应调节**：  
   - 灯光色温与亮度智能调节  
   - 模拟电器（空调/风扇/照明）自动控制  


## **技术栈**  
### 硬件  
- Arduino MEGA2560  
- MAX30100心率传感器  
- WS2812全彩LED  
- DHT11温湿度传感器  
- 光敏电阻传感器  
- 5V继电器模块  

### 软件  
- Python 3.8+  
- MediaPipe（表情识别）  
- OpenCV（图像处理）  
- Arduino IDE/PlatformIO  
- FastLED库、DHT库、MAX30105库  


## **安装与配置**  
### 硬件连接  
1. **MAX30100传感器**：  
   ```  
   VCC → Arduino 5V（串联1kΩ分压电阻）  
   GND → Arduino GND  
   SDA → Arduino A4  
   SCL → Arduino A5  
   ```  

2. **WS2812 LED**：  
   ```  
   VCC → Arduino 5V  
   GND → Arduino GND  
   DATA → Arduino D6（串联220Ω限流电阻）  
   ```  

3. **继电器模块**：  
   ```  
   IN1 → Arduino D3（空调模拟）  
   IN2 → Arduino D4（灯光模拟）  
   IN3 → Arduino D5（风扇模拟）  
   ```  

### 软件安装  
1. **电脑端环境**：  
   ```bash  
   # 克隆项目  
   git clone https://github.com/Cavalcdor/Smart_Environment_Control.git
   cd Smart_Environment_Control/Software/Computer  
   # 安装Python依赖  
   pip install -r Requirements.txt  
   ```  

2. **Arduino端环境**：  
   - 安装Arduino IDE或PlatformIO  
   - 安装以下库：  
     ```  
     FastLED  
     DHT sensor library  
     MAX30105 Particle Sensor  
     ```  


## **使用方法**  
1. **启动Arduino端程序**：  
   - 打开 `Software/Arduino/Src/Main.ino`  
   - 上传至Arduino MEGA2560  
   - 打开串口监视器（波特率115200）确认初始化成功  

2. **启动电脑端程序**：  
   ```bash  
   cd Smart_Environment_Control/Software/Computer/Src  
   python Main.py  
   ```  

3. **系统运行**：  
   - 摄像头自动启动并检测面部表情  
   - MAX30100传感器实时采集心率数据  
   - 系统根据多模态数据自动调节灯光和电器状态  


## **配置参数**  
修改 `Config/Settings.json` 可调整系统行为：  
```json  
{  
    "Bluetooth": {  
        "Port": "COM3",         // 蓝牙串口号（Windows）/dev/ttyUSB0（Linux）  
        "BaudRate": 9600        // 波特率  
    },  
    "Sensors": {  
        "DHT11_Pin": 2,         // DHT11数据引脚  
        "Light_Sensor_Pin": "A0" // 光敏传感器引脚  
    },  
    "Control": {  
        "Temp_High_Threshold": 28,  // 高温触发阈值（℃）  
        "Temp_Low_Threshold": 22,   // 低温触发阈值（℃）  
        "HR_High_Threshold": 90,    // 高心率触发阈值（次/分）  
        "HR_Low_Threshold": 70      // 低心率触发阈值（次/分）  
    }  
}  
```  


## **系统测试**  
1. **单元测试**：  
   ```bash  
   cd Smart_Environment_Control/Tests/Unit_Tests  
   python test_emotion_detection.py  
   python test_heart_rate_processing.py  
   ```  

2. **集成测试**：  
   ```bash  
   cd Smart_Environment_Control/Tests/Integration_Tests  
   python test_system_response.py  
   ```  


## **技术原理**  
1. **表情识别**：  
   - 基于MediaPipe面部网格模型，提取68个关键点坐标  
   - 通过嘴角与下眼睑的垂直距离比例判断表情状态（微笑/皱眉/中性）  

2. **心率处理**：  
   - 采用10点滑动平均滤波消除高频噪声  
   - 通过峰值检测算法计算心率值及变异性（HRV）  

3. **模糊控制**：  
   - 建立4×3×3维规则表（情绪×心率×温度）  
   - 采用Mamdani推理法实现模糊化、规则推理和解模糊  


## **贡献指南**  
1. **问题反馈**：  
   - 提交Issue时请注明环境（硬件版本/软件版本）和复现步骤  

2. **代码贡献**：  
   -  Fork项目并创建功能分支（`git checkout -b feature/new-function`）  
   - 提交前确保代码通过单元测试（`pytest Tests/Unit_Tests`）  
   - 提交Pull Request时需附带文档说明  


## **开发者**  
- **开发者**：Charles WENG
- **邮箱**：Hegongyu007@zju.edu.cn
