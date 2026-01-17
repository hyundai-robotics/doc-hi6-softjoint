# 2. Commands

The SoftJoint function is configured and controlled using two commands:  
`softjoint_lim` and `softjoint`.

- The `softjoint_lim` command is used to **predefine the base parameters** required for SoftJoint operation.
- The `softjoint` command **enables or disables** the SoftJoint function based on the parameters configured by `softjoint_lim`.

Therefore, to use the SoftJoint function, the user must first configure the target joint and its motion characteristics using the `softjoint_lim` command, and then enable the function by executing the `softjoint on` command.

Using the `softjoint_lim` command, the user can define the following items:
- Joint index to which SoftJoint will be applied
- Level of compliant response to external forces
- Allowable joint angle range
- Threshold value for external force detection