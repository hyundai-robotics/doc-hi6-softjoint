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