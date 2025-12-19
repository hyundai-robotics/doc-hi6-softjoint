# ${cont_model} Robot Controller Function Description – SoftJoint Function

The information provided in this product manual is the property of **HD Hyundai Robotics**.

Without prior written consent from HD Hyundai Robotics, this document, in whole or in part,  
may not be reproduced, redistributed, provided to any third party, or used for any other purpose.

The contents of this manual are subject to change without prior notice.

<br>
<br>
<br>
<br>

**Copyright ⓒ 2025 by HD Hyundai Robotics**# 🧩 1. Overview

The Soft Joint function allows the robot to respond flexibly to external forces in the joint coordinate frame, based on the environment configured by the user.
To ensure accurate operation of this function, information regarding the tool mounted on the robot or any additional payload must be configured correctly.

Since this function operates entirely through software, it can be used without any additional hardware, such as force/torque sensors.

---
# 🧩 2. Commands

The SoftJoint function is configured and controlled using two commands:  
`softjoint_lim` and `softjoint`.

- The `softjoint_lim` command is used to **predefine the base parameters** required for SoftJoint operation.
- The `softjoint` command **enables or disables** the SoftJoint function based on the parameters configured by `softjoint_lim`.

Therefore, to use the SoftJoint function, the user must first configure the target joint and its motion characteristics using the `softjoint_lim` command, and then enable the function by executing the `softjoint on` command.

Using the `softjoint_lim` command, the user can define the following items:
- Joint index to which SoftJoint will be applied
- Level of compliant response to external forces
- Allowable joint angle range
- Threshold value for external force detection## 🧩 2.1 softjoint

The SoftJoint function allows the robot to respond compliantly to external forces in the joint coordinate frame without the use of sensors.

<br>

### Syntax

```plaintext
softjoint on
softjoint off
```

---

### Parameters

| Parameter | Description |
|-----|------|
| on  | Enables the SoftJoint function |
| off | Disables the SoftJoint function |

---
<br>

> ✅ **Information**  
> Before using the `softjoint on` command, you must configure the following parameters in advance using the `softjoint_lim` command:
>
> - Joint index to be compliant (`j`)
> - Compliance level (`sft`)
> - Limit angle (`ang`)
> - Threshold value (`thr`)
>
> To improve the robot’s sensitivity to external forces,  
> it is recommended to stop the robot for approximately **1–2 seconds**  
> using the `delay` command before executing the `softjoint on` command.

<br>

> ⚠️ **Warning**  
> This function is **not supported for auxiliary axes**. ## 🧩 2.2 softjoint_lim 

The `softjoint_lim` command is used to **preconfigure the required parameters** for SoftJoint operation before enabling the `softjoint on` function.  
Using this command, the user must set the following items in advance:

- Joint index to which SoftJoint will be applied
- Compliance level of the joint
- Allowable joint angle range
- Threshold value for external force detection

<br>

### Syntax

```plaintext
softjoint_lim, j=<joint>, sft=<softness>, ang=<angle>, thr=<threshold>
```

---

### Parameters

| Parameter | Description | Range / Unit |
|-----------|-------------|--------------|
| `j`   | Joint index to which the SoftJoint function is applied | Robot joints only |
| `sft` | Joint compliance level (higher values result in more compliant behavior) | 0: Off, 1–100 |
| `ang` | Allowable joint angle range | Degrees (deg) |
| `thr` | Threshold value for external force detection | Nm |
---
<br>

> ✅ **Information**  
> For the `softjoint_lim` command, the joint index (`j`) and the compliance level (`sft`) **must be specified**.  
> If the angle range (`ang`) and threshold value (`thr`) are not provided,  
> the angle limit will not be applied and the threshold value will be automatically set to **0.0 Nm**.

> ⚠️ **Warning**  
> This function is **not supported for auxiliary axes**.## 🧩 3. Examples

This section provides **representative configuration and program examples** using the  
`softjoint_lim` and `softjoint` commands to help users understand how to use the SoftJoint function in practice.

Each example is designed to demonstrate the SoftJoint behavior under different parameter settings,  
including the target joint, compliance level, angle limit, and threshold value.  
The examples are structured with consideration for **practical application in real working environments**.

Through these examples, users can learn the following:
- How to apply SoftJoint to a single joint
- How to apply different parameters to multiple joints
- Program flow when enabling and disabling the SoftJoint function
- The recommended usage procedure, including the `delay` command
## 🧩 3.1 Example: Parameter Configuration for Joint 3

This example shows how to enable SoftJoint for **Joint 3** with the following settings:
- **Active joint**: Joint 3
- **Compliance (`sft`)**: 50
- **Angle limit (`ang`)**: ±30 deg
- **Threshold (`thr`)**: 10 Nm

```plaintext
softjoint_lim, j=3, sft=50, ang=30, thr=10## 🧩 3.2 Example: Configuration Allowing Compliance on Joints 2 and 3

This example demonstrates how to apply different SoftJoint parameters to **Joint 2** and **Joint 3**,  
so that each joint responds to external forces with different characteristics.

#### Configuration Conditions

- **Compliance (`sft`)**
  - Joint 2: 30
  - Joint 3: 80

- **Angle Limit (`ang`)**
  - Joint 2: −50° to +50°
  - Joint 3: No limit

- **Threshold (`thr`)**
  - Joint 2: 3 Nm
  - Joint 3: 5 Nm

#### Program Example

```plaintext
S1   move P, spd=100mm/sec, accu=0, tool=0
     delay 2.0     # Delay required before executing softjoint on
     softjoint_lim j=2, sft=30, ang=50, thr=3
     softjoint_lim j=3, sft=80, thr=5
     softjoint on

S2   move P, spd=250mm/sec, accu=0, tool=0
     softjoint off
     end