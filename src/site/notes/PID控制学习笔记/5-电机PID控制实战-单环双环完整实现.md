---
{"dg-publish":true,"permalink":"/pid/5-pid/","noteIcon":""}
---

# 电机 PID 控制实战：从速度单环到角度双环

## 目录

- [1. M3508 速度单环 PID 控制](#1-m3508-速度单环-pid-控制)
- [2. GM6020 角度双环 PID 控制](#2-gm6020-角度双环-pid-控制)
- [3. PID 结构体设计与算法实现](#3-pid-结构体设计与算法实现)
- [4. 嵌入式 C 编程核心要点](#4-嵌入式-c-编程核心要点)
- [5. 工程实践总结](#5-工程实践总结)

---

## 1. M3508 速度单环 PID 控制

### 1.1 控制目标与系统特点

- **控制目标**：精确控制 M3508 电机的转速（RPM）
- **系统特点**：
  - 减速比 19:1（电机转 19 圈，输出轴转 1 圈）
  - 采用单环控制结构，直接对速度进行 PID 调节
  - 输出为电流指令，直接发送给电调

### 1.2 核心算法实现

速度单环 PID 控制采用**积分分离**和**积分限幅**技术，以平衡响应速度与稳定性。

```c
// PID 控制器结构体定义
typedef struct {
    float target;          // 目标值
    float current;         // 当前值
    float e;               // 当前误差
    float last_e;          // 上次误差
    
    float Kp, Ki, Kd;     // PID 参数
    float p_out, i_out, d_out; // 各分量输出
    float total_out;       // 总输出
    
    // 高级功能参数
    float I_Band;          // 积分分离阈值
    float IntegralLimit;   // 积分限幅值
    float MaxOutput;       // 总输出限幅
} pid_t;

// PID 计算函数
void pid_calc(pid_t* pid) {
    pid->e = pid->target - pid->current;
    
    // 1. 比例项
    pid->p_out = pid->Kp * pid->e;
    
    // 2. 积分项（带积分分离）
    if (fabs(pid->e) <= pid->I_Band) {
        // 误差在积分带内：启用积分
        pid->i_out += pid->Ki * pid->e;
        
        // 积分限幅
        if (pid->i_out > pid->IntegralLimit)
            pid->i_out = pid->IntegralLimit;
        else if (pid->i_out < -pid->IntegralLimit)
            pid->i_out = -pid->IntegralLimit;
    } else {
        // 误差过大：禁用积分，防止积分饱和
        pid->i_out = 0;
    }
    
    // 3. 微分项
    pid->d_out = pid->Kd * (pid->e - pid->last_e);
    
    // 4. 合并输出并限幅
    pid->last_e = pid->e;
    pid->total_out = pid->p_out + pid->i_out + pid->d_out;
    
    if (pid->total_out > pid->MaxOutput)
        pid->total_out = pid->MaxOutput;
    else if (pid->total_out < -pid->MaxOutput)
        pid->total_out = -pid->MaxOutput;
}
```

### 1.3 积分分离与限幅的作用

1. **积分分离 (I_Band)**：
   - **问题**：当目标速度大幅跳变时，初始误差极大，若立即投入积分，会导致积分项快速累积，引起严重超调和震荡。
   - **解决方案**：设置误差阈值 `I_Band`，仅当 `|e| ≤ I_Band` 时才启用积分。
   - **效果**：大幅跳跃时靠 P 项快速响应；接近目标时靠 I 项消除静差。

2. **积分限幅 (IntegralLimit)**：
   - **问题**：长时间存在误差时，积分项可能累积至极大值（Windup），即使误差反向，仍需长时间"倒车"才能恢复正常。
   - **解决方案**：对积分累加值施加硬性上下限。
   - **效果**：防止系统"刹不住车"，提高抗饱和能力。

---

## 2. GM6020 角度双环 PID 控制

### 2.1 控制系统架构

![rm电机6020角度双环.png](/img/user/PID%E6%8E%A7%E5%88%B6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/picture/rm%E7%94%B5%E6%9C%BA6020%E8%A7%92%E5%BA%A6%E5%8F%8C%E7%8E%AF.png)

针对 GM6020 云台电机的角度控制，系统采用**串级控制（Cascade Control）**结构：

- **外环：位置环 (Position Loop)**
  - **输入**：设定的目标角度（度）
  - **控制器**：位置 PID
  - **输出**：作为内环的期望速度

- **内环：速度环 (Speed Loop)**
  - **输入**：来自位置环的期望速度
  - **控制器**：速度 PID
  - **输出**：最终输出给电调的电流指令

- **传感器反馈**：
  - **位置**：编码器数据处理后的累加角度
  - **速度**：电调回传的实时转速

### 2.2 角度更新与编码器处理

GM6020 使用单圈绝对值编码器（0～8191），需处理多圈旋转和"过零跳变"问题。

```c
typedef struct {
    uint16_t encoder;          // 当前编码器值
    uint16_t last_encoder;     // 上次编码器值
    uint16_t encoder_offset;   // 初始位置偏移
    int16_t round_cnt;         // 圈数计数
    uint8_t encoder_is_init;   // 初始化标志
    int32_t total_encoder;     // 总脉冲数
    float angle;              // 计算出的角度（度）
} motor_angle;

void update_angle(motor_angle* _angle, uint16_t angle_fb) {
    _angle->encoder = angle_fb; // 读取当前编码器值
    
    if (_angle->encoder_is_init) {
        // 判断是否发生过零跳变（以半圈幅值4096为界）
        if (_angle->encoder - _angle->last_encoder > 4096)
            _angle->round_cnt--; // 反向过零
        else if (_angle->encoder - _angle->last_encoder < -4096)
            _angle->round_cnt++; // 正向过零
    } 
    else {
        _angle->encoder_offset = _angle->encoder; // 记录初始位置
        _angle->encoder_is_init = 1;
    }
    
    _angle->last_encoder = _angle->encoder;
    
    // 计算总脉冲数并转换为角度（8192脉冲 = 360度）
    _angle->total_encoder = _angle->round_cnt * 8192 + _angle->encoder - _angle->encoder_offset;
    _angle->angle = _angle->total_encoder / 8192.0f * 360.0f;
}
```

### 2.3 双环级联调用逻辑

```c
// 1. 获取并处理反馈
update_angle(&gm6020_angle, feedback.encoder);

// 2. 外环计算：位置环
angle_pid.target = 30.0f; // 目标角度：30度
angle_pid.current = gm6020_angle.angle;
pid_calc(&angle_pid); 

// 3. 关键：将外环输出传给内环目标
speed_pid.target = angle_pid.total_out; 

// 4. 内环计算：速度环
speed_pid.current = feedback.speed_rpm;
pid_calc(&speed_pid);

// 5. 最终执行
Set_Motor_Current(speed_pid.total_out);
```

### 2.4 串级控制的优势与调参原则

1. **串级控制的本质**：
   - 外环负责"指方向"（确定目标位置）
   - 内环负责"干苦力"（快速跟踪速度指令）
   - 内环的存在极大提高系统抗扰能力

2. **调参顺序**：**必须先调内环，再调外环**
   - 只有内环（速度环）响应足够快且稳定，外环（位置环）的控制才有物理基础
   - 内环调参：先调 P 到临界震荡，再加 D 抑制超调，最后加小 I 消除稳态误差
   - 外环调参：基于稳定内环，逐步增大 P 提高响应速度

3. **积分策略差异**：
   - **外环（位置环）**：通常使用较小的 $K_i$ 甚至不使用积分，主要依靠 $K_p$ 快速对齐位置
   - **内环（速度环）**：配合积分分离和积分限幅，确保能快速抵消摩擦力和负载干扰

---

## 3. PID 结构体设计与算法实现

### 3.1 结构体设计哲学

嵌入式系统中的 PID 控制器设计遵循 **"数据与逻辑分离"** 原则：

```c
// 数据：全部封装在结构体中
typedef struct {
    // 输入/状态
    float target, current, e, last_e;
    
    // 参数
    float Kp, Ki, Kd;
    
    // 中间计算结果
    float p_out, i_out, d_out;
    
    // 输出
    float total_out;
    
    // 高级功能参数
    float I_Band, IntegralLimit, MaxOutput;
} pid_t;

// 逻辑：纯函数，只操作传入的结构体指针
void pid_calc(pid_t* pid) {
    // 算法实现...
}
```

**设计优势**：
1. **高内聚**：所有相关数据集中管理
2. **低耦合**：算法函数不依赖全局变量
3. **易复用**：可实例化多个独立的 PID 控制器
4. **可维护**：参数调整只需修改结构体成员

### 3.2 算法实现的工程考量

1. **数据类型选择**：
   - 浮点数（`float`）：精度高，但计算速度较慢
   - 定点数：速度快，但需要手动管理小数位
   - 根据处理器性能和精度要求选择

2. **采样周期处理**：
   - 离散积分：$K_i = K_p \cdot \frac{\Delta t}{T_i}$
   - 离散微分：$K_d = K_p \cdot \frac{T_d}{\Delta t}$
   - 需确保采样周期 $\Delta t$ 固定

3. **抗积分饱和的两种策略**：
   ```c
   // 方法1：积分限幅（硬限制）
   if (pid->i_out > pid->IntegralLimit) pid->i_out = pid->IntegralLimit;
   
   // 方法2：积分冻结（当输出饱和时停止积分）
   if (fabs(pid->total_out) >= pid->MaxOutput && pid->e * pid->total_out > 0) {
       // 输出已饱和且误差方向与输出相同：停止积分
   }
   ```

---

## 4. 嵌入式 C 编程核心要点

### 4.1 结构体与类型定义

```c
// 类型定义：创建"图纸"
typedef struct {
    float x, y, z;
} Vector3;

// 变量声明：按照图纸"盖房子"
Vector3 position;  // 创建一个Vector3类型的变量

// 访问成员
position.x = 10.0f;
float current_x = position.x;
```

### 4.2 指针操作三要素

1. **声明指针**：`类型* 指针名`
   ```c
   pid_t* pid_ptr;  // 指向pid_t类型的指针
   ```

2. **获取地址**：`&变量`
   ```c
   pid_t my_pid;
   pid_ptr = &my_pid;  // 获取my_pid的地址
   ```

3. **解引用**：`*指针` 或 `指针->成员`
   ```c
   (*pid_ptr).Kp = 1.0f;  // 传统写法
   pid_ptr->Kp = 1.0f;    // 箭头写法（推荐）
   ```

### 4.3 内存生命周期管理

| **存储类型** | **生命周期** | **初始化** | **典型用途** |
|-------------|-------------|-----------|-------------|
| **自动变量** | 函数执行期间 | 未初始化（随机值） | 临时计算、循环计数器 |
| **静态局部** | 整个程序运行期 | 自动清零 | 保持状态的计数器、标志位 |
| **全局变量** | 整个程序运行期 | 自动清零 | 系统配置、共享数据 |

### 4.4 函数设计最佳实践

1. **纯函数设计**：
   ```c
   // 好：输入明确，输出明确，无副作用
   float calculate_pid(pid_t* pid, float target, float current);
   
   // 差：依赖全局状态，难以调试
   float target, current;
   float calculate_pid_bad();  // 从哪里读target和current？
   ```

2. **错误处理**：
   ```c
   typedef enum {
       PID_OK = 0,
       PID_ERROR_NULL_PTR,
       PID_ERROR_INVALID_PARAM
   } pid_status_t;
   
   pid_status_t pid_calc_safe(pid_t* pid) {
       if (!pid) return PID_ERROR_NULL_PTR;
       if (pid->Kp < 0) return PID_ERROR_INVALID_PARAM;
       
       pid_calc(pid);
       return PID_OK;
   }
   ```

---

## 5. 工程实践总结

### 5.1 单环 vs 双环选择原则

| **特性** | **速度单环控制** | **角度双环控制** |
|---------|----------------|----------------|
| **适用场景** | 对位置精度要求不高，主要关注速度稳定 | 需要精确位置控制，如云台、机械臂 |
| **系统复杂度** | 简单，只有一个控制回路 | 复杂，需协调两个耦合的控制器 |
| **抗扰能力** | 一般，直接受负载变化影响 | 强，内环可快速抑制速度波动 |
| **调参难度** | 较低，只有一组 PID 参数 | 较高，需先调内环再调外环 |
| **实时性要求** | 中等（通常 100-500Hz） | 高（内环需 1kHz 以上） |

### 5.2 参数整定经验值

**M3508 速度环参考参数**（基于 1kHz 控制频率）：
- $K_p$: 0.5～2.0
- $K_i$: 0.01～0.1（配合积分分离）
- $K_d$: 0.05～0.2
- 积分分离阈值：目标速度的 20%～30%

**GM6020 双环参考参数**：
- **内环（速度）**：
  - $K_p$: 8.0～15.0
  - $K_i$: 0.5～2.0（必须限幅）
  - $K_d$: 0.1～0.5
- **外环（位置）**：
  - $K_p$: 10.0～30.0
  - $K_i$: 0～0.5（可省略）
  - $K_d$: 1.0～5.0

### 5.3 调试与故障排查

1. **系统完全不响应**：
   - 检查硬件连接（编码器、电调）
   - 验证控制频率是否正常
   - 确认 PID 输出限幅合理

2. **电机震荡**：
   - 降低 $K_p$ 或 $K_d$
   - 检查积分分离阈值是否过小
   - 确认反馈信号无噪声干扰

3. **响应迟缓**：
   - 增大 $K_p$
   - 检查积分限幅是否过小
   - 确认控制频率足够高

4. **稳态误差大**：
   - 适当增大 $K_i$（需配合积分限幅）
   - 检查积分分离阈值是否过大

### 5.4 高级优化方向

1. **自适应 PID**：根据误差大小动态调整参数
2. **前馈补偿**：加入速度前馈或加速度前馈提高跟踪性能
3. **陷波滤波**：抑制机械谐振频率的影响
4. **模糊 PID**：对非线性系统提供更好的控制效果

> [!NOTE] **关键工程建议**
> 
> 1. **采样频率**：控制频率至少为系统带宽的 10 倍以上
> 2. **实时性**：使用 RTOS 确保定时器中断的精确性
> 3. **安全保护**：必须实现输出限幅和积分限幅
> 4. **监控调试**：预留调试接口，实时观测 PID 各分量输出
> 5. **文档记录**：详细记录参数调整过程和最终效果

---

**总结**：电机 PID 控制是嵌入式系统开发的核心技能。从简单的速度单环到复杂的角度双环，关键在于理解 PID 的物理本质、掌握结构化的代码实现方法，并遵循科学的调试流程。通过不断实践和经验积累，才能在各种应用场景中实现精准、稳定的电机控制。