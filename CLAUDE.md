# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ROS2 (ament_cmake) robot description package for the Mulinex family of robots: a quadrupedal platform with swappable feet (Otto/normal, OmniQuad/omni, GoatQuad/goat) and a wheeled omnidirectional variant (OmniCar with mecanum wheels).

## Build & Run

```shell
# Build (from workspace root, typically one level above this package)
colcon build --packages-select mulinex_description

# Visualize URDF in RViz (loads omnicar.xacro)
ros2 launch mulinex_description display_mulinex.launch.py

# Simulate in Gazebo (loads mulinex.xacro)
ros2 launch mulinex_description gazebo_mulinex.launch.py

# Lint (configured but cpplint is disabled)
colcon test --packages-select mulinex_description
```

Runtime dependencies not in this package: `xacro`, `robot_state_publisher`, `joint_state_publisher`, `joint_state_publisher_gui`, `rviz2`, `gazebo_ros`, `gz_ros2_control`.

## Architecture

### URDF/Xacro hierarchy

Two independent entry points:

- **mulinex.xacro** — Quadruped. Includes `links.xacro` (base geometry), `leg.xacro` (leg macro), `material.xacro`. The `feet_type` arg (`normal`|`omni`|`goat`|`track`) selects foot geometry. ROS2 control: position commands on HFE/KFE joints (8 DOF, HAA joints currently disabled).
- **omnicar.xacro** — Wheeled variant. Includes `omni_car_module.xacro`, `wheel.xacro`, sensor xacros (IMU, LiDAR, camera), `omnicar_ignition.xacro` for Ignition Gazebo control. ROS2 control: velocity commands on 4 wheel joints.

Shared building blocks: `base_inertia.xacro` (inertia helpers), `material.xacro` (color definitions), `hip_module.xacro` (shoulder/hip assembly).

### Naming conventions

- Leg prefixes: `LF` (left-front), `LH` (left-hind), `RF` (right-front), `RH` (right-hind)
- Joint types: `HAA` (hip abduction-adduction), `HFE` (hip flexion-extension), `KFE` (knee flexion-extension)
- Dimensions defined as xacro properties in mm, then converted to meters

### Key directories

- `urdf/` — All xacro files (robot structure, sensors, control)
- `meshes/` — STL files; `simplify_meshes.py` generates convex-hull collision meshes via pymeshlab
- `launch/` — ROS2 Python launch files
- `world/` — Gazebo/Ignition world files (empty, gap, obstacles)
- `rviz/` — RViz config

### External dependencies

This package references config from **mulinex_ignition** (e.g., `omnicar.yaml` controller config, `mulinex_mf.yaml` for Gazebo plugins). That package must be present in the workspace for simulation.
