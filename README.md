# RoboRacer Control Module

This repository documents my contribution to the **RoboRacer Autonomous Racing Project**, focused on the **Control** module.

The original team repository is private and the full project is developed collaboratively across multiple branches. My work is developed on the **Control branch**, where I focus on trajectory tracking, speed control, command generation, and debugging tools for the autonomous racing vehicle.

> **Note:** This repository currently contains documentation of my contribution. Source files may be added later only if sharing is approved by the project team and supervisors.

---

## Project Context

RoboRacer is a team-based autonomous racing project using a ROS2-based software stack for a scaled autonomous racecar.

The full system is organized into four main modules:

- **Perception**
- **Estimation**
- **Planning**
- **Control**

My responsibility is the **Control** module.

The goal of the Control module is to receive the planned path and vehicle state estimate, then generate safe and stable steering and speed commands for the vehicle in simulation and later on the physical platform.

---

## My Role

**Role:** Control Module Developer

My main tasks include:

- Implementing path tracking for the autonomous racecar
- Generating Ackermann steering and speed commands
- Integrating the controller with the existing ROS2 command-routing pipeline
- Using odometry feedback for closed-loop control
- Computing tracking errors for evaluation
- Adding RViz visualization tools for debugging
- Validating the controller in simulation before hardware deployment

---

## Control Architecture

The controller is designed to operate inside the team’s ROS2 autonomy stack.

```text
Planning module → /plan → Pure Pursuit Controller → /nav → mux → /drive → simulator / vehicle
                                      ↑
                                    /odom
```

### Main Inputs

| Topic | Message Type | Description |
|---|---|---|
| `/plan` | `nav_msgs/msg/Path` | Planned trajectory from the Planning module |
| `/odom` | `nav_msgs/msg/Odometry` | Vehicle pose and velocity estimate |

### Main Outputs

| Topic | Message Type | Description |
|---|---|---|
| `/nav` | `ackermann_msgs/msg/AckermannDriveStamped` | Steering and speed command from the controller |
| `/tracking_error` | `geometry_msgs/msg/Vector3Stamped` | Cross-track and heading error output |
| `/control_debug_markers` | `visualization_msgs/msg/MarkerArray` | RViz markers for debugging |

The final command is routed through the existing mux system and executed on `/drive`.

---

## Controller Overview

The Control module is implemented as a path-tracking controller using:

- **Pure Pursuit** for lateral control
- **PID speed control** for longitudinal control
- **Ackermann command publishing** for steering and velocity commands
- **Odometry feedback** for closed-loop behavior

---

## Lateral Control: Pure Pursuit

The Pure Pursuit controller tracks the planned path by selecting a lookahead target point and computing the required steering angle.

At each control cycle, the controller:

1. Receives the current path from `/plan`
2. Receives vehicle pose from `/odom`
3. Finds the nearest relevant point on the path
4. Selects a lookahead target point
5. Transforms the target into the vehicle frame
6. Computes the steering command
7. Applies steering limits before publishing the command

This allows the vehicle to continuously follow the planned trajectory.

---

## Longitudinal Control: PID Speed Tracking

The longitudinal controller uses a PID-based approach to regulate vehicle speed.

It compares the reference speed with the measured velocity from odometry and computes a corrected speed command.

The speed behavior includes:

- Nominal forward speed tracking
- Speed reduction in sharp turns
- Speed reduction near the goal
- Smooth stopping after reaching the goal
- Output limiting for safer command generation

---

## Tracking Error Computation

The controller computes tracking errors to evaluate path-following performance.

| Error | Description |
|---|---|
| Cross-track error | Lateral distance between the vehicle and the reference path |
| Heading error | Angular difference between vehicle heading and path direction |

The closest reference point is computed using projection onto path segments, which gives a more accurate error measurement than simply comparing the vehicle to the lookahead target.

---

## RViz Debug Visualization

The controller publishes debug markers to make the internal control behavior visible in RViz.

The visualization includes:

- Lookahead target point
- Lookahead circle
- Steering direction arrow
- Error vector from the vehicle to the closest path point

These markers are useful for checking whether the controller is selecting the correct target, steering in the expected direction, and tracking the path accurately.

---

## Forward and Reverse Motion Handling

The controller supports both forward and reverse motion.

The selected target point is transformed into the local vehicle frame:

- `x_local > 0` → target is in front of the vehicle
- `x_local < 0` → target is behind the vehicle

A hysteresis-based switching logic is used to reduce unstable switching between forward and reverse modes.

---

## Current Status

Current implemented features include:

- ROS2 Control node for path tracking
- Pure Pursuit steering control
- PID-based speed control
- Ackermann command generation
- Integration with `/plan`, `/odom`, and `/nav`
- Cross-track error computation
- Heading error computation
- RViz debug markers
- Forward and reverse motion support
- Goal detection and stopping behavior
- Simulation-based validation

---

## Simulation Workflow

The controller is designed to run with the team’s F1TENTH/RoboRacer simulator setup.

### Terminal 1 — Start Simulator

```bash
cd ~/ros2_ws
source install/setup.bash
ros2 launch f1tenth_simulator simulator.launch.py
```

### Terminal 2 — Start Navigation

```bash
cd ~/ros2_ws
source install/setup.bash
ros2 launch f1tenth_simulator navigation.launch.py
```

### Terminal 3 — Start RViz

```bash
ros2 run rviz2 rviz2 -d /opt/ros/humble/share/nav2_bringup/rviz/nav2_default_view.rviz
```

### Activate Navigation Mode

```bash
ros2 topic pub --once /key std_msgs/msg/String "data: 'n'"
```

---

## Useful Debug Commands

```bash
ros2 topic echo /plan
ros2 topic echo /odom
ros2 topic echo /nav
ros2 topic echo /tracking_error
```

---

## Technologies Used

- ROS2 Humble
- C++
- Ackermann steering model
- Pure Pursuit control
- PID control
- Odometry feedback
- RViz visualization
- F1TENTH / RoboRacer simulation environment

---

## Future Work

Planned improvements include:

- Higher-speed controller tuning
- Steering command smoothing
- Steering rate limiting
- Curvature-based feedforward steering
- Watchdog-based fail-safe behavior
- Hardware calibration
- Real-track testing on the physical RoboRacer platform

---

## Repository Status

This repository is intended to present my work on the Control branch of the private team project.

At the moment, it may only contain documentation. The implementation files can be added later after permission is confirmed.

---

## Author

**Mohammad Barabadi**  
Control Module Developer  
RoboRacer Autonomous Racing Project
