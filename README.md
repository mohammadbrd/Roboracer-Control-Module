# RoboRacer Control Module

My control-module contribution to RoboRacer-Shiran, a five-member autonomous racing project at FAU Erlangen-Nürnberg. I developed the ROS 2 controller and tested it in simulation and on a physical F1TENTH race car.

This repository documents my contribution and recorded results. The full project source code is maintained in the public team repository.

- [Team repository](https://github.com/farhadvaseghi/RoboRacer-Shiran)

## My Contribution

- Developed the C++ ROS 2 control module to generate Ackermann steering and speed commands from a planned path and vehicle feedback.
- Implemented Pure Pursuit path tracking and tuned a speed-dependent lookahead.
- Implemented odometry-feedback PID speed tracking as a configurable option.
- Calculated cross-track and heading errors and added RViz markers for debugging.
- Debugged waypoint tracking and bounded the forward path search to reduce incorrect jumps to later path segments.
- Adapted and tested the controller on the physical F1TENTH vehicle.

The wider team developed the other parts of the autonomy stack, including perception, state estimation, mapping, and navigation planning. The linked implementation reflects the team's shared development.

## Control Approach

The controller receives a planned path and vehicle state, selects a lookahead target, and computes the steering angle using Pure Pursuit. It publishes steering and speed commands using `AckermannDriveStamped` messages.

Cross-track and heading errors support performance evaluation. RViz markers show the selected target, lookahead geometry, and tracking error.

PID speed feedback can be enabled through `use_speed_pid`. The reviewed team simulator configuration sets it to `false`, because the simulator already controls the requested vehicle speed internally.

## Recorded Hardware Test

The following results describe one recorded run on the physical vehicle.

| Metric | Recorded value |
| --- | --- |
| Distance estimated by onboard localization | 31.56 m |
| Run duration | 22.1 s |
| Maximum commanded speed | 1.5 m/s |
| Median absolute cross-track error | 0.095 m |
| 95th-percentile absolute cross-track error | 0.242 m |
| Final estimated distance to goal | 0.189 m |
| Configured goal tolerance | 0.25 m |

Measurement basis: onboard localization, without independent ground-truth validation. These values describe this run and should be interpreted with that measurement basis.

## Technologies

ROS 2 Humble, C++, Pure Pursuit, PID, Ackermann steering, odometry, RViz, and F1TENTH simulation and hardware.

## Academic Context

FAU Erlangen-Nürnberg, M.Sc. Autonomy Technologies. Completed as the Team Project / Industriepraktikum module, 10 ECTS, passed.

## Author

[Mohammad Barabadi](https://github.com/mohammadbrd) | Control Module Developer
