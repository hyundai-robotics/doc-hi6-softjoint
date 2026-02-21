
[__SOURCE](README.md)
# ${cont_model} 控制器功能描述 - SoftJoint 功能
[__SOURCE](0-about-this-manual/precautions.md)
# 注意事项

{% include url="https://hrcontentsrelay-bmgae5hdbzapc4bc.koreacentral-01.azurewebsites.net/api/proxy?path=doc-common-pages/en/precautions.md" %}
[__SOURCE](1-intro/README.md)
# 1. 概述

软关节功能允许机器人在关节坐标系中灵活响应外部力量，基于用户配置的环境。  
为了确保该功能的准确操作，必须正确配置关于安装在机器人上的工具或任何额外负载的信息。  

由于该功能完全通过软件运行，因此可以在没有任何额外硬件的情况下使用，例如力/扭矩传感器。
[__SOURCE](2-main/README.md)
# 2. 命令

SoftJoint 功能通过两个命令进行配置和控制：  
`softjoint_lim` 和 `softjoint`。

- `softjoint_lim` 命令用于 **预定义 SoftJoint 操作所需的基础参数**。
- `softjoint` 命令 **根据 `softjoint_lim` 配置的参数启用或禁用** SoftJoint 功能。

因此，要使用 SoftJoint 功能，用户必须首先使用 `softjoint_lim` 命令配置目标关节及其运动特性，然后通过执行 `softjoint on` 命令启用该功能。

使用 `softjoint_lim` 命令，用户可以定义以下项目：
- 应用 SoftJoint 的关节索引
- 对外部力量的顺应响应水平
- 允许的关节角度范围
- 外部力量检测的阈值
[__SOURCE](2-main/2.1-softjoint.md)
## 2.1 softjoint

SoftJoint 功能允许机器人在关节坐标框架中对外部力做出顺应反应，而无需使用传感器。

<br>

### 语法

```plaintext
softjoint on
softjoint off
```

---

### 参数

| 参数     | 描述                         |
|----------|------------------------------|
| on       | 启用 SoftJoint 功能         |
| off      | 禁用 SoftJoint 功能         |

---
<br>

> ✅ **信息**  
> 在使用 `softjoint on` 命令之前，您必须提前使用 `softjoint_lim` 命令配置以下参数：
>
> - 需要顺应的关节索引 (`字母j (j)`)
> - 顺应级别 (`sft`)
> - 限制角度 (`ang`)
> - 阈值 (`thr`)
>
> 为了提高机器人对外部力的敏感性，  
> 建议在执行 `softjoint on` 命令之前，使用 `延迟 (delay)` 命令使机器人停止约 **1-2秒**。

<br>

> ⚠️ **警告**  
> 此功能 **不支持辅助轴**。
[__SOURCE](2-main/2.2-softjoint_lim.md)
## 2.2 softjoint_lim 

`softjoint_lim` 命令用于 **预配置 SoftJoint 操作所需的参数**，在启用 `softjoint on` 功能之前。  
使用该命令时，用户必须提前设置以下项目：

- 应用 SoftJoint 的关节索引
- 关节的顺应性级别
- 允许的关节角度范围
- 外部力检测的阈值

<br>

### 语法

```plaintext
softjoint_lim, j=<joint>, sft=<softness>, ang=<angle>, thr=<threshold>
```

---

### 参数

| 参数 | 描述 | 范围 / 单位 |
|-----------|-------------|--------------|
| `字母j (j)`   | 应用 SoftJoint 功能的关节索引 | 仅限机器人关节 |
| `sft` | 关节顺应性级别（较高的值导致更顺应的行为） | 0: 关闭, 1-100 |
| `ang` | 允许的关节角度范围 | 度 (deg) |
| `thr` | 外部力检测的阈值 | Nm |
---
<br>

> ✅ **信息**  
> 对于 `softjoint_lim` 命令，关节索引 (`字母j (j)`) 和顺应性级别 (`sft`) **必须指定**。  
> 如果未提供角度范围 (`ang`) 和阈值 (`thr`)，  
> 则不会应用角度限制，阈值将自动设置为 **0.0 Nm**。

> ⚠️ **警告**  
> 此功能 **不支持辅助轴**。
[__SOURCE](3-example/README.md)
# 3. 示例

本节提供 **使用 `softjoint_lim` 和 `softjoint` 命令的代表性配置和程序示例**，帮助用户理解如何在实践中使用 SoftJoint 功能。

每个示例旨在展示在不同参数设置下的 SoftJoint 行为，包括目标关节、柔顺级别、角度限制和阈值。示例结构考虑到 **在实际工作环境中的应用**。

通过这些示例，用户可以学习到以下内容：
- 如何将 SoftJoint 应用于单个关节
- 如何将不同的参数应用于多个关节
- 启用和禁用 SoftJoint 功能时的程序流程
- 推荐的使用程序，包括 `延迟 (delay)` 命令
[__SOURCE](3-example/3.1-example.md)
## 3.1 示例：关节 3 的参数配置

此示例演示如何使用以下设置启用 **关节 3** 的 SoftJoint：
- **有效关节**：关节 3
- **柔顺性 (`sft`)**：50
- **角度限制 (`ang`)**：±30 度
- **阈值 (`thr`)**：10 Nm

```plaintext
softjoint_lim, j=3, sft=50, ang=30, thr=10
```
[__SOURCE](3-example/3.2-example.md)
## 3.2 示例：允许关节 2 和 3 的柔顺配置

本示例演示了如何将不同的 SoftJoint 参数应用于 **关节 2** 和 **关节 3**，  
使每个关节以不同的特性响应外部力。

#### 配置条件

- **柔顺性 (`sft`)**
  - 关节 2: 30
  - 关节 3: 80

- **角度限制 (`ang`)**
  - 关节 2: -50° 到 +50°
  - 关节 3: 无限制

- **阈值 (`thr`)**
  - 关节 2: 3 Nm
  - 关节 3: 5 Nm

#### 程序示例

```plaintext
S1   move P, spd=100mm/sec, accu=0, tool=0
     delay 2.0     # 执行软关节之前所需的延迟
     softjoint_lim j=2, sft=30, ang=50, thr=3
     softjoint_lim j=3, sft=80, thr=5
     softjoint on

S2   move P, spd=250mm/sec, accu=0, tool=0
     softjoint off
     end
```