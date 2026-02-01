# robotiq noetic-devel

用于 Robotiq C-Model 系列夹爪/控制器的 ROS 元包，提供基于 Modbus 的 TCP 与 RTU（串口）控制节点、消息与示例启动文件。适用于如 Robotiq 2F-85 + K-1363 控制器等设备，也可与 UR e-series 上的 Hand-e 使用。

## 仓库说明

- `robotiq_control`：C-Model 夹爪控制节点（TCP/RTU），以及 action/simple controller。
- `robotiq_msgs`：夹爪指令与状态消息。
- `robotiq_description`：URDF/Xacro 描述与可视化模型。
- `robotiq_ft_sensor`：力传感器（如 FTS150）相关节点与驱动。

## 安装与编译

以下以 `catkin_ws` 为例：

```bash
cd ~/catkin_ws/src
git clone [https://github.com/cambel/robotiq.git](https://github.com/Tingfengyue/robotiq.git)
```

安装依赖：

```bash
rosdep update
rosdep install --from-paths . --ignore-src -y
```

编译：

```bash
cd ~/catkin_ws
catkin_make
```

使用前记得 source：

```bash
source ~/catkin_ws/devel/setup.bash
```

## 使用方式

### Modbus TCP（以 C-Model 控制器为例）

```bash
roslaunch robotiq_control cmodel_simple_controller.launch ip:=ROBOTIQ_IP_ADDRESS
```

或使用 action controller：

```bash
roslaunch robotiq_control cmodel_action_controller.launch \
  use_tcp_control_mode:=true \
  ip:=ROBOTIQ_IP_ADDRESS
```

### Modbus RTU（串口直连）

```bash
roslaunch robotiq_control cmodel_action_controller.launch \
  use_tcp_control_mode:=false \
  device_name:=/dev/ttyUSB0
```

> 串口设备名以你的实际设备为准（如 `/dev/ttyUSB0`、`/dev/ttyS0`）。

## 安装成功的验证命令

运行简单控制器并观察终端输出：

```bash
roslaunch robotiq_control cmodel_simple_controller.launch ip:=ROBOTIQ_IP_ADDRESS
```

出现如下交互提示即表示节点启动成功：

```
Simple C-Model Controller
-----
Current command:  rACT = 0, rGTO = 0, rATR = 0, rPR = 0, rSP = 0, rFR = 0
-----
Available commands
...
-->
```

如需完整例子（UR3e + Gazebo），可参考 `https://github.com/cambel/ur3`。
