# RoboRacer Control Module

My control-module contribution to RoboRacer-Shiran, a five-member autonomous racing project at FAU Erlangen-Nürnberg. I developed the ROS 2 controller and tested it in simulation and on a physical F1TENTH race car.

This repository documents my contribution to the team project. The full source code, technical documentation, and test reports are available in the public team repository.

- [Team repository](https://github.com/farhadvaseghi/RoboRacer-Shiran)

## My Contribution

- Developed the C++ ROS 2 control module to generate Ackermann steering and speed commands from a planned path and vehicle feedback.
- Implemented Pure Pursuit path tracking and tuned a speed-dependent lookahead.
- Implemented odometry-feedback PID speed tracking as a configurable option.
- Calculated cross-track and heading errors and added RViz markers for debugging.
- Debugged waypoint tracking and bounded the forward path search to reduce incorrect jumps to later path segments.
- Adapted and tested the controller on the physical F1TENTH vehicle.

The wider team developed the other parts of the autonomy stack, including perception, state estimation, mapping, and navigation planning. The linked implementation reflects the team's shared development.

## Technologies

ROS 2 Humble, C++, Pure Pursuit, PID, Ackermann steering, odometry, RViz, and F1TENTH simulation and hardware.

## Academic Context

FAU Erlangen-Nürnberg, M.Sc. Autonomy Technologies. Completed as the Team Project / Industriepraktikum module, 10 ECTS, passed.

## Author

[Mohammad Barabadi](https://github.com/mohammadbrd) | Control Module Developer
