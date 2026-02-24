# ros2_cmd_vel_splitter

## Overview
`ros2_cmd_vel_splitter` is a minimal `rclpy` node that listens to a single velocity command stream and republishes each `geometry_msgs/Twist` message to multiple destinations. Targets are runtime-configurable via ROS 2 parameters or a YAML config. The node source lives in [ros2_cmd_vel_splitter/cmd_vel_splitter_node.py](ros2_cmd_vel_splitter/cmd_vel_splitter_node.py).

## Installation
### Dependencies
- ROS 2 (rclpy, geometry_msgs available in your distro)

### Build
```bash
colcon build --packages-select ros2_cmd_vel_splitter --symlink-install
source install/setup.bash
```

## Usage
Run with the provided defaults:
```bash
ros2 run ros2_cmd_vel_splitter cmd_vel_splitter \
  --ros-args --params-file $(ros2 pkg prefix ros2_cmd_vel_splitter)/share/ros2_cmd_vel_splitter/config/cmd_vel_splitter.yaml
```

Update topics at runtime:
```bash
ros2 param set /cmd_vel_splitter input_topic /cmd_vel
ros2 param set /cmd_vel_splitter output_topics "['/mcb7/cmd_vel','/mcb31/cmd_vel']"
ros2 param set /cmd_vel_splitter queue_depth 20
```

## Nodes
### `cmd_vel_splitter`
**Subscribed Topics**
| Topic | Type | Description |
| :--- | :--- | :--- |
| `input_topic` (parameter) | `geometry_msgs/Twist` | Incoming velocity command (default `/cmd_vel`). |

**Published Topics**
| Topic | Type | Description |
| :--- | :--- | :--- |
| `output_topics` (parameter list) | `geometry_msgs/Twist` | Fan-out destinations (default `/mcb7/cmd_vel`, `/mcb31/cmd_vel`). |

**Parameters**
| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `input_topic` | string | `/cmd_vel` | Topic to subscribe for velocity commands. |
| `output_topics` | string array | `[/mcb7/cmd_vel, /mcb31/cmd_vel]` | One or more topics to republish. |
| `queue_depth` | int | `10` | QoS queue depth for sub and pubs (must be >= 1). |

## Configuration
Default parameters are in [config/cmd_vel_splitter.yaml](config/cmd_vel_splitter.yaml). Adjust this file or override with `ros2 param set`.

## Repository Pointers
- Package metadata: [package.xml](package.xml)
- Setup: [setup.py](setup.py), [setup.cfg](setup.cfg)
- Resource index: [resource/ros2_cmd_vel_splitter](resource/ros2_cmd_vel_splitter)
- Node implementation: [ros2_cmd_vel_splitter/cmd_vel_splitter_node.py](ros2_cmd_vel_splitter/cmd_vel_splitter_node.py)