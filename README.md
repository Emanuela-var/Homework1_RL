# homework1_rl2024
Homework 1 for Robotics Lab 2024/2025

First build all the packages by using:

```
colcon build --packages-select arm_gazebo arm_description arm_control arm_controller
```
In each terminal you open, source the install directory:
```
source install/setup.bash
```

# 1. Create the description of your robot and visualize it in Rviz
To launch `display.launch.py`, run:
```
ros2 launch arm_description display.launch.py
```
It automatically loads the `arm.rviz` configuration located in `arm_description/config/rviz`. To visualize the collision boxes, they need to be enabled in the left sidebar. 
The `joint_publisher_gui` sliders are launched within `display.launch.py`.

# 2. Add sensors and controllers to your robot and spawn it in Gazebo
To start Gazebo and spawn the robot, run:
```
ros2 launch arm_gazebo arm_world.launch.py
```
This command will not spawn the controllers, because they are launched in `arm_control.launch.py`, in the `arm_control` package.

To spawn the robot in Gazebo and load the controllers, run:
```
ros2 launch arm_gazebo arm_gazebo.launch.py
```
In another terminal, the joint states can be monitored by running:
```
ros2 topic echo /joint_states
```
You can test the position controllers by publishing onto the `\position_controller\commands` topic from terminal. For example:
```
ros2 topic pub /position_controller/commands std_msgs/msg/Float64MultiArray "data: [0.0, 0.03, 0.2, 0.1]"
```

# 3. Add a camera sensor to your robot
With the robot already spawned in Gazebo by launching `arm_gazebo.launch.py`, you can see the image published by the camera by running, in a different terminal:
```
ros2 topic pub /position_controller/commands std_msgs/msg/Float64MultiArray "data: [0.0, 0.03, 0.2, 0.1]"
```
You can see the image by running:
```
rqt
```
In the rqt window, go to Plugins > Visualization > Image View to load the `rqt_image_view` plugin. You need to select the `/videocamera` topic.

# 4. Create a ROS publisher node that reads the joint state and sends joint position commands to your robot
With the robot already spawned in Gazebo by launching `arm_gazebo.launch.py`, you can run the `arm_controller_node`. The argument specifies the desired joint positions. The node automatically prints the joint states in the terminal. An example is:
```
ros2 run arm_controller arm_controller_node --ros-args -p joint_positions:="[0.3, 0.0, 0.02, 0.3]"
```
If no argument is specified, the node publishes the default message `joint_positions:="[0.0, 0.0, 0.0, 0.0]`.
