
[__SOURCE](README.md)
# ${cont_model} 控制器功能描述 - SoftJoint 功能
[__SOURCE](0-about-this-manual/README.md)
# 关于手册
[__SOURCE](0-about-this-manual/precautions.md)
# 注意事项

{% include file="zh/precautions.md" %}
[__SOURCE](0-about-this-manual/safety-notice.md)
# 安全注意事项

{% include file="zh/safety-notice.md" %}
[__SOURCE](1-intro/README.md)
# 1. 概述

**软关节功能**允许机器人根据用户配置的环境，在**关节坐标系**中灵活地响应外部力量。
为了确保此功能的准确操作，必须正确配置关于**安装在机器人上的工具**或**任何额外负载**的信息。

由于此功能完全通过**软件**操作，因此可以**在没有任何额外硬件（如力/扭矩传感器）的情况下使用。**
[__SOURCE](2-main/README.md)
# 2. 命令

SoftJoint 功能是通过两个命令配置和控制的:  
`softjoint_lim` 和 `softjoint`。

- `softjoint_lim` 命令用于 **预定义 SoftJoint 操作所需的基本参数**。
- `softjoint` 命令 **根据 `softjoint_lim` 配置的参数启用或禁用** SoftJoint 功能。

因此，要使用 SoftJoint 功能，用户必须首先使用 `softjoint_lim` 命令配置目标关节及其运动特性，然后通过执行 `softjoint on` 命令启用该功能。

使用 `softjoint_lim` 命令，用户可以定义以下项:
- 将应用 SoftJoint 的关节索引
- 对外部力量的顺应响应级别
- 允许的关节角度范围
- 外部力量检测的阈值
[__SOURCE](2-main/2.1-softjoint.md)
## 2.1 softjoint

`SoftJoint` 功能允许机器人在 **关节坐标系** 中对外部力量 **作出顺应性反应**，无需使用传感器。

<br>

### 语法

```plaintext
softjoint on
softjoint off
```

---

### 参数

| 参数 | 描述 |
|-----|------|
| on  | 启用 SoftJoint 功能 |
| off | 禁用 SoftJoint 功能 |

---
<br>

{% hint style="info" %}
在使用 `softjoint on` 命令之前，您必须使用 `softjoint_lim` 命令提前配置以下参数：

* 要顺应的关节索引 (` (j)`)
* 顺应级别 (`sft`)
* 限制角度 (`ang`)
* 门限值 (`thr`)

为了提高机器人对外部力量的敏感度，建议在执行 `softjoint on` 命令之前，使用 `时间延迟 (delay)` 命令使机器人停止约 **1-2 秒**。
{% endhint %}
<br>

{% hint style="warning" %}

此功能 **不支持辅助轴**。 
{% endhint %}
[__SOURCE](2-main/2.2-softjoint_lim.md)
## 2.2 softjoint_lim 

The `softjoint_lim` command is used to **预配置所需参数** for SoftJoint operation before enabling the **softjoint on** function.  
Using this command, the user must set the following items in advance:

* 应用SoftJoint的关节索引
* 关节的顺应级别
* 允许的关节角度范围
* 外部力检测的阈值

---
<br>

### Syntax

```plaintext
softjoint_lim, j=<joint>, sft=<softness>, ang=<angle>, thr=<threshold>
```

---

### Parameters

| Parameter | Description | Range / Unit |
|-----------|-------------|--------------|
| ` (j)`   | 应用SoftJoint功能的关节索引 | 仅限机器人关节 |
| `sft` | 关节顺应级别（更高的值会导致更符合的行为） | 0: 关闭, 1-100 |
| `ang` | 允许的关节角度范围 | 度（deg） |
| `thr` | 外部力检测的阈值 | Nm |
---
<br>

{% hint style="info" %}

* For the `softjoint_lim` command, the joint index (` (j)`) and the compliance level (`sft`) **必须指定**.  
* If the angle range (`ang`) and threshold value (`thr`) are not provided,  
* the angle limit will not be applied and the threshold value will be automatically set to **0.0 Nm**.
{% endhint %}

{% hint style="warning" %}

This function is **不支持辅助轴**.
{% endhint %}
[__SOURCE](3-example/README.md)
# 3. 示例

本节提供 **代表性的配置和程序示例**，使用  
`softjoint_lim` 和 `softjoint` 命令，以帮助用户理解如何在实践中使用 SoftJoint 功能。

每个示例旨在展示在不同参数设置下 SoftJoint 的行为，  
包括目标关节、合规等级、角度限制和阈值。  
这些示例的结构考虑了 **在实际工作环境中的应用**。

通过这些示例，用户可以学习以下内容：
- 如何将 SoftJoint 应用于单个关节
- 如何将不同参数应用于多个关节
- 启用和禁用 SoftJoint 功能时的程序流程
- 推荐的使用程序，包括 `时间延迟 (delay)` 命令
[__SOURCE](3-example/3.1-example.md)
## 3.1 示例：关节 3 的参数配置

此示例**仅激活关节 3 轴**，并演示一个基本案例，其中**该轴根据指定的柔顺特性对外部力做出响应。**

---

#### 配置概述
对于第三轴，**柔性、角度限制和阈值已根据单轴参考进行配置**，以清晰定义对外部力的响应范围。

---
#### 配置条件

* **激活关节**：关节 3
* **柔顺性 (`sft`)**：50
* **角度限制 (`ang`)**：±30 度
* **阈值 (`thr`)**：10 Nm

---
#### 程序示例

```plaintext
softjoint_lim, j=3, sft=50, ang=30, thr=10
```
[__SOURCE](3-example/3.2-example.md)
## 3.2 示例：配置允许关节 2 和 3 的柔顺性

本示例演示如何将不同的 SoftJoint 参数应用于 **关节 2** 和 **关节 3**，  
使每个关节对外部力量的响应具有不同的特性。

---

#### 配置概述
对于每个关节轴，**柔软度、角度限制和阈值独立配置**，允许对每个轴的外部力响应特性进行精确控制。

---

#### 配置条件

* **柔顺性 (`sft`)**
  * 关节 2: 30 (相对较硬的响应)
  * 关节 3: 80 (更灵活的响应)

* **角度限制 (`ang`)**
  * 关节 2: -50° 到 +50°
  * 关节 3: 无限制

* **阈值 (`thr`)**
  * 关节 2: 3 Nm
  * 关节 3: 5 Nm

---

#### 程序示例

```plaintext
S1   move P, spd=100mm/sec, accu=0, tool=0
     delay 2.0     # 在执行软关节之前所需的延迟
     softjoint_lim j=2, sft=30, ang=50, thr=3
     softjoint_lim j=3, sft=80, thr=5
     softjoint on

S2   move P, spd=250mm/sec, accu=0, tool=0
     softjoint off
     end
```