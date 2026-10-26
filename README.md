# Franka Panda Robotic Manipulation 🤖

**ROS 2 Jazzy · Gazebo Sim 8 · MoveIt 2 · ros2_control**

A simulation-based robotics project integrating a **7-degree-of-freedom Franka Emika Panda manipulator** with ROS 2, Gazebo Sim, joint trajectory control, and MoveIt 2 motion planning. The project provides a foundation for developing and evaluating robotic manipulation workflows in simulation.

> **Project status:** Gazebo simulation, arm/gripper controllers, joint trajectory execution, and interactive MoveIt 2 planning have been demonstrated. Autonomous pick-and-place is planned future work.

## Features

- 7-DOF Panda arm with two-finger gripper described using URDF/Xacro.
- Robot spawning and visualization in Gazebo Sim 8.
- Gazebo-to-ROS 2 control integration through `gz_ros2_control`.
- `joint_state_broadcaster`, `arm_controller`, and `gripper_controller` configured and activated.
- Joint-space trajectory execution using `FollowJointTrajectory` actions.
- MoveIt 2 integration for interactive planning in RViz 2.

## Technology stack

| Component | Technology |
| --- | --- |
| Operating system | Ubuntu 24.04 |
| ROS | ROS 2 Jazzy |
| Simulator | Gazebo Sim 8 |
| Motion planning | MoveIt 2 + RViz 2 |
| Robot modeling | URDF / Xacro |
| Control | `ros2_control`, `gz_ros2_control` |
| Controllers | Joint State Broadcaster, Joint Trajectory Controllers |

## Workspace structure

The project is developed in `~/panda_ws`. Key source locations include:

```text
panda_ws/
└── src/
    ├── panda_description/
    │   ├── launch/
    │   │   └── gazebo.launch.py
    │   └── urdf/
    │       ├── panda.urdf.xacro
    │       ├── arm.xacro
    │       ├── gazebo.xacro
    │       └── ros2control.xacro
    └── panda_controller/
        ├── launch/
        │   └── panda_controller.launch.py
        └── config/
            └── controllers.yaml
```

*This tree highlights known key files rather than claiming to list every file in the repository.*

## Build and launch

### Prerequisites

Install ROS 2 Jazzy, Gazebo Sim 8, MoveIt 2, and the appropriate Jazzy-compatible `ros2_control` / `gz_ros2_control` packages. Ensure all project dependencies are available before building.

### Build the workspace

```bash
cd ~/panda_ws
source /opt/ros/jazzy/setup.bash
colcon build --symlink-install
source install/setup.bash
```

### Launch the simulation

In **Terminal 1**:

```bash
source /opt/ros/jazzy/setup.bash
source ~/panda_ws/install/setup.bash
ros2 launch panda_description gazebo.launch.py
```

In **Terminal 2**, launch the controllers if they are not already started by your simulation launch file:

```bash
source /opt/ros/jazzy/setup.bash
source ~/panda_ws/install/setup.bash
ros2 launch panda_controller panda_controller.launch.py
```

> The exact launch order and MoveIt 2 launch command depend on the configuration files in your checkout. Confirm them against the repository before running multiple controller managers or launching MoveIt 2.

## Verify controllers

```bash
ros2 control list_controllers
```

The demonstrated setup reported all three controllers as **active**:

```text
joint_state_broadcaster  joint_state_broadcaster/JointStateBroadcaster          active
arm_controller          joint_trajectory_controller/JointTrajectoryController  active
gripper_controller       joint_trajectory_controller/JointTrajectoryController  active
```

Inspect joint states:

```bash
ros2 topic echo /joint_states --once
```

## Joint trajectory control

The project successfully sent trajectory goals to:

- `/arm_controller/follow_joint_trajectory` for Panda joints 1–7.
- `/gripper_controller/follow_joint_trajectory` for the two finger joints.

A demonstrated arm target was:

```text
[0.2, 0.0, 0.0, -1.5708, 0.0, 1.5708, 0.7854]
```

The gripper was commanded to:

```text
[0.04, 0.04]
```

These are example joint targets from simulation, not a general-purpose safe motion sequence for physical hardware.

## Motion planning with MoveIt 2

MoveIt 2 has been brought up with the Panda robot in RViz 2. The MotionPlanning panel and interactive target markers were available, and a planning request succeeded.

**Demonstrated:** Interactive target selection and successful planning.

**Next validation step:** Record a repeatable planned-trajectory execution and document the launch command and planning group used.

## Screenshots and demo

Add project media to a `docs/images/` directory and link it here:

- Gazebo simulation of the Panda arm.
- RViz 2 MotionPlanning interface with interactive markers.
- Controller-status terminal output.
- Short demonstration of trajectory execution.

## Roadmap

- [x] Build Panda robot description in URDF/Xacro.
- [x] Launch the Panda robot in Gazebo Sim.
- [x] Configure ROS 2 arm and gripper controllers.
- [x] Execute joint trajectory action goals.
- [x] Integrate MoveIt 2 and verify interactive planning.
- [ ] Document repeatable MoveIt 2 plan-and-execute workflow.
- [ ] Add collision objects and a manipulation scene.
- [ ] Implement autonomous pick-and-place.
- [ ] Evaluate grasp planning and task success across object poses.
- [ ] Explore perception-guided or learning-based manipulation.

## Repository

[Franka-Panda-Robotic-Manipulation](https://github.com/Tulasi-Pottella/Franka-Panda-Robotic-Manipulation)

## Author

**Pottella Tulasi**  
B.Tech, Mechanical Engineering · NIT Durgapur  
Interests: Robotics Software, Motion Planning, Robot Control, and Embodied AI.

---

*This repository documents simulation and software development. Physical robot deployment requires additional safety checks and hardware-specific validation.*
