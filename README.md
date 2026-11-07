# ROS Assignment 5 — TurtleBot3 Simulations

**Ubuntu 26.04 | ROS 2 Lyrical | Gazebo Jetty | TurtleBot3 Waffle Pi**

This README documents how I adapted [ROS 2 Workshop Tutorial 5](https://github.com/thillRobot/ros2_workshop) for my Ubuntu 26.04 VMware virtual machine. The original assignment targets Ubuntu 24.04, ROS 2 Jazzy, and Gazebo Harmonic; I retained my working ROS 2 Lyrical installation and used Gazebo Jetty.

**Result:** Gazebo opened, the TurtleBot3 house simulation loaded, and I successfully moved the robot using keyboard controls.

## Step 1 — Verify ROS 2 Lyrical

Open an Ubuntu terminal.

**1. Source the ROS environment:**

```bash
source /opt/ros/lyrical/setup.bash
```

**2. Check the ROS version:**

```bash
echo $ROS_DISTRO
```

Expected output: `lyrical`.

**3. Refresh the package list:**

```bash
sudo apt update
```

**4. Check available packages:**

```bash
apt-cache policy ros-lyrical-ros-gz ros-lyrical-turtlebot3-simulations ros-lyrical-turtlebot3-msgs ros-lyrical-turtlebot3
```

**What happened:** The Gazebo, TurtleBot3 simulation, and message packages were available. The all-in-one `ros-lyrical-turtlebot3` package was unavailable, but the simulation could still be installed.

## Step 2 — Install and test Gazebo

**1. Install the Gazebo integration:**

```bash
sudo apt install ros-lyrical-ros-gz
```

**2. Configure software rendering for VMware:**

```bash
echo 'export LIBGL_ALWAYS_SOFTWARE=1' >> ~/.bashrc
```

**3. Reload shell configuration:**

```bash
source ~/.bashrc
```

**4. Start Gazebo:**

```bash
gz sim
```

Choose a built-in world and select **Run**.

**My result:** Gazebo opened successfully. Close the test simulation before proceeding.

> If the prompt doesn't return after running Gazebo, press **Ctrl+C** in its terminal or open another terminal with **Ctrl+Alt+T**. I encountered this when trying to enter `sudo apt update`.

## Step 3 — Install TurtleBot3 simulation packages

**1. Install the packages:**

```bash
sudo apt install ros-lyrical-turtlebot3-simulations ros-lyrical-turtlebot3-msgs
```

**2. Verify the simulation package:**

```bash
ros2 pkg prefix turtlebot3_gazebo
```

Expected output is a package prefix such as `/opt/ros/lyrical`.

**My result:** The simulation packages installed successfully.

## Step 4 — Choose TurtleBot3 Waffle Pi

**1. Save the robot model selection:**

```bash
echo 'export TURTLEBOT3_MODEL=waffle_pi' >> ~/.bashrc
```

**2. Reload the configuration:**

```bash
source ~/.bashrc
```

**3. Confirm the setting:**

```bash
echo $TURTLEBOT3_MODEL
```

Expected output: `waffle_pi`.

> Only append each `export` line once to `~/.bashrc`; running the same `echo ... >> ~/.bashrc` command repeatedly creates duplicates.

## Step 5 — Launch the house simulation

Open **Terminal 1** and leave it running.

**1. Source ROS:**

```bash
source /opt/ros/lyrical/setup.bash
```

**2. Set the robot model:**

```bash
export TURTLEBOT3_MODEL=waffle_pi
```

**3. Launch the house world:**

```bash
ros2 launch turtlebot3_gazebo turtlebot3_house.launch.py
```

**Expected result:** Gazebo opens a house scene containing the TurtleBot3 robot.

## Step 6 — Install keyboard controls

The professor's tutorial uses `turtlebot3_teleop`, but ROS 2 Lyrical repository did not have that package.

**1. Check availability of the original controller:**

```bash
apt-cache policy ros-lyrical-turtlebot3-teleop
```

**Observed output:** `N: Unable to locate package ros-lyrical-turtlebot3-teleop`.

**2. Install the alternative controller:**

```bash
sudo apt install ros-lyrical-teleop-twist-keyboard
```

**3. Open Terminal 2** while keeping Gazebo running in Terminal 1.

**4. Source ROS in Terminal 2:**

```bash
source /opt/ros/lyrical/setup.bash
```

**5. Run the controller:**

```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -p stamped:=true
```

The `stamped:=true` parameter selects stamped velocity commands for this setup.

**result:** The controller launched and TurtleBot3 moved in response to keyboard commands.

## Step 7 — Drive the robot inside the house

Keep **Terminal 2 selected** to receive keyboard input.

| Key | Function |
| --- | --- |
| `i` | Forward |
| `,` | Backward |
| `j` | Turn left |
| `l` | Turn right |
| `u` / `o` | Forward while turning |
| `k` or Space | Stop |
| Ctrl+C | Exit the controller |

Use the above keys to drive TurtleBot3 into the house and then press `k` to stop.

## Troubleshooting notes from the ChatGPT discussion

This is a **condensed record of our troubleshooting**, not a verbatim chat transcript.

| Issue | Resolution |
| --- | --- |
| Tutorial was designed for Ubuntu 24.04/Jazzy | Continued with Ubuntu 26.04/Lyrical; did not run the Kilted-to-Jazzy conversion script |
| Uncertain package availability | Used `apt-cache policy` to verify Gazebo and simulations packages |
| Terminal would not accept `sudo apt update` | Used Ctrl+C to return to the command prompt |
| Gazebo installation needed checking | Ran `gz sim` successfully |
| Needed a house world | Launched `turtlebot3_house.launch.py`; house and robot appeared |
| `ros-lyrical-turtlebot3-teleop` unavailable | Installed `ros-lyrical-teleop-twist-keyboard` |
| Needed keyboard movement | Ran `teleop_twist_keyboard` with `stamped:=true`; TurtleBot3 moved |

## Conclusion

I completed the TurtleBot3 simulation portion of this assignment using my existing ROS 2 Lyrical installation rather than switching to Jazzy. The main compatibility issue was the missing TurtleBot3-specific keyboard package. Using the general ROS 2 keyboard controller allowed me to drive the robot in the Gazebo house world successfully.

## References

- [ROS 2 Workshop](https://github.com/thillRobot/ros2_workshop)
- [ROS 2 Lyrical documentation](https://docs.ros.org/en/lyrical/)
- [Gazebo Jetty documentation](https://gazebosim.org/docs/jetty/)


## Relevant public tutorial

- Workshop repository: <https://github.com/thillRobot/ros2_workshop>

This repo summarizes the **user's actual Lyrical adaptation**, not the original workflow through Jazzy.

