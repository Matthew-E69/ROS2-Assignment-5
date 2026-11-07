# ROS Assignment 5 — TurtleBot3 in Gazebo (Ubuntu 26.04 / ROS 2 Lyrical)

This repository documents a **working adaptation** of ROS Assignment 5 / Tutorial 5 from the `ros2_workshop` to an existing Ubuntu 26.04 VMware VM with **ROS 2 Lyrical**.

> **Status:** Gazebo runs, the TurtleBot3 house simulation loads, and a keyboard controller successfully moves TurtleBot3. The final screenshot (robot inside house **and** keyboard terminal) still needs to be provided. The included PDF is a draft with a screenshot placeholder, **not a final submission**.

## Why this adaptation was needed

The professor's tutorial uses Ubuntu 24.04, ROS 2 Jazzy, and Gazebo Harmonic. The existing VM uses Ubuntu 26.04, ROS 2 Lyrical, and Gazebo Jetty. The `kilted_to_jazzy.sh` conversion script was **not** run because it was made for a different operating-system/ROS setup and is unnecessary when the existing installation works.

## Verified on this VM

| Item | Observed result |
| --- | --- |
| `ros-lyrical-ros-gz` | Package available; installed; `gz sim` successfully launched |
| `ros-lyrical-turtlebot3-simulations` | Available; house and robot launched successfully |
| `ros-lyrical-turtlebot3-msgs` | Available |
| `ros-lyrical-turtlebot3` | Not found by apt |
| `ros-lyrical-turtlebot3-teleop` | Not found by apt |
| `ros-lyrical-teleop-twist-keyboard` | Alternative keyboard controller worked; TurtleBot3 moved |

These results reflect the user's local apt configuration when the assignment was completed; availability can change.

## Run the assignment again

**Terminal 1: Launch TurtleBot3 house**

```bash
source /opt/ros/lyrical/setup.bash
export TURTLEBOT3_MODEL=waffle_pi
ros2 launch turtlebot3_gazebo turtlebot3_house.launch.py
```

**Terminal 2: Keyboard control**

```bash
source /opt/ros/lyrical/setup.bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -p stamped:=true
```

Keep Terminal 2 selected when typing. In `teleop_twist_keyboard`, `i` moves forward, `j`/`l` turn, and `k` stops. `u`/`o` move while turning. Press Ctrl+C to exit.

If the teleop node does not move the robot on a different installation, inspect the velocity topic, publisher/subscriber connections, and expected Twist versus TwistStamped message type before changing parameters.

## Installation commands used

```bash
source /opt/ros/lyrical/setup.bash
sudo apt update
sudo apt install ros-lyrical-ros-gz
# Software OpenGL was selected for the VMware guest:
echo 'export LIBGL_ALWAYS_SOFTWARE=1' >> ~/.bashrc
source ~/.bashrc
# GUI smoke test:
gz sim
```

Close the basic Gazebo test before running the TurtleBot3 house world.

```bash
sudo apt install ros-lyrical-turtlebot3-simulations ros-lyrical-turtlebot3-msgs
# Add only if not already set in ~/.bashrc:
echo 'export TURTLEBOT3_MODEL=waffle_pi' >> ~/.bashrc
source ~/.bashrc
ros2 pkg prefix turtlebot3_gazebo
```

```bash
sudo apt install ros-lyrical-teleop-twist-keyboard
```

**Note:** The above `echo ... >> ~/.bashrc` commands append lines. If repeated, manually remove duplicate lines when convenient. Some installations may load the ROS environment automatically from `~/.bashrc`.

## Relevant public tutorial

- Workshop repository: <https://github.com/thillRobot/ros2_workshop>

This repo summarizes the **user's actual Lyrical adaptation**, not the original workflow through Jazzy.

