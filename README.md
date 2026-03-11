# Mulinex Robots Description

ROS2 package with URDF descriptions for the Mulinex robot family.

## Robots

### Mulinex (`urdf/mulinex.xacro`) — Quadruped

| Parameter | Values | Default | Description |
|---|---|---|---|
| `feet_type` | `normal`, `omni`, `goat`, `track` | `omni` | Foot type (Otto, OmniQuad, GoatQuad, track) |
| `simplify_meshes_lower_leg` | `true`, `false` | `true` | Use convex-hull collision meshes for shanks |
| `use_gazebo` | `true`, `false` | `false` | Include Gazebo plugins and ros2_control |
| `jnt_prefix` | string | `""` | Prefix for all joint/link names |
| `{LF,LH,RF,RH}_{HFE,KFE}` | float (rad) | `0.0` | Initial joint positions |

### OmniCar (`urdf/omnicar.xacro`) — Wheeled (mecanum)

| Parameter | Values | Default | Description |
|---|---|---|---|
| `use_gazebo` | `true`, `false` | `false` | Include Ignition Gazebo plugins and ros2_control |
| `yaml_file` | path | `""` | Controller config YAML (from `mulinex_ignition`) |
| `lidar_3D` | `true`, `false` | `true` | 3D LiDAR (900x15 rays) vs 2D (360 rays) |
| `jnt_prefix` | string | `""` | Prefix for all joint/link names |

Sensors: IMU (100 Hz), LiDAR (50 Hz), RGBD camera (30 Hz, 320x240).

## Usage

Visualize in RViz
```shell
ros2 launch mulinex_description display_mulinex.launch.py
```

Simulate in Gazebo
```
ros2 launch mulinex_description gazebo_mulinex.launch.py
```
