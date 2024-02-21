# Development Environment

## System Requirements

Autoware imposes strict requirement on system environment. It requires
specific operating system, specific CUDA version and specific ROS
version. Be careful to set up your box.

- Ubuntu 22.04
- ROS Humble + Cyclone DDS as default RMW back-end
- CUDA 12.3
- CUDNN 8.9.5.29-1+cuda12.2
- TensorRT 8.6.1.6-1+cuda12.0

In the case that the box does not support required OS or NVIDIA driver
is not available. One workaround is to start an Ubuntu 22.04 container
and put Autoware into it.


## Autoware Source Code Layout

Autoware source code consists of many GitHub repositories. The
top-level repository is
[autowarefoundation/autoware](https://github.com/autowarefoundation/autoware),
which does not include any source code. It has a `autoware.repos` file
listing dependent repos. Run this command to download these repositories

```bash
vcs import < autoware.repos
```

Note that `vcs` does not provide fine-grained version control. Hence,
downloaded repos do not pin to specific Git commits, but tracks to
update-to-date master or main branch.

Read `autoware.repos` and you can see the packages classified as
follows.

- [core](https://github.com/autowarefoundation/autoware.core)
  - Contains the most important package for self-driving cars and
    determine the message field type and field name for services.
- [universe](https://github.com/autowarefoundation/autoware.universe)
  - Packages developed by other third-party communities.
- [launcher](https://github.com/autowarefoundation/autoware_launch)
  - A launch configuration repository for Autoware, containing node
    configurations and their parameters, where the launch files will be.
- [sensor\_component](https://autowarefoundation.github.io/autoware-documentation/main/design/autoware-architecture/sensing/)
  - Sensing component is a collection of modules that applies some
    primitive pre-processing to the raw sensor data. The sensor input
    formats are defined in this component.
- [sensor\_kit](https://github.com/autowarefoundation/sample_sensor_kit_launch)
  - Launch each sensors according to the parameters.
- [vehicle](https://github.com/autowarefoundation/sample_vehicle_launch)
  - Launch each vehicles according to the parameters.
- [param](https://github.com/autowarefoundation/sample_vehicle_launch)
  - This repository stores parameters that change depending on each
    vehicle.

## The Autoware as a ROS Project

The packaging and execution guideline for Autoware follow ROS
convention. The packages can be categorized as follows.

1. Message definition
2. Launcher packages
3. Parameters
4. Nodes and execution units

The message definition files, launchers and parameters are packed in
handful of packages. The node/execution packages comprises the major
parts of Autoware. They are grouped into perception, planning and many
other components. Explore the node
[diagram](https://autowarefoundation.github.io/autoware-documentation/main/design/autoware-architecture/node-diagram/overall-node-diagram-autoware-universe.drawio.svg)
to discover the nodes and topic connections among them.


### The Launch System

The
[autoware\_launch](https://github.com/autowarefoundation/autoware_launch/tree/main/autoware_launch/launch)
package contains the launch file `autoware.launch.xml` as the entry
file. It loads children launch files, such as
[sample\_sensor\_kit\_launch](https://github.com/autowarefoundation/sample_sensor_kit_launch)
and
[sample\_vehicle\_launch](https://github.com/autowarefoundation/sample_vehicle_launch)
to start subsystems.

Pass `map_path`, `vehicle_model` and `sensor_model` arguments to
launch the Autoware.

```bash
ros2 launch autoware_launch autoware.launch.xml \
    map_path:=$HOME/autoware_map/sample-map-planning \
    vehicle_model:=sample_vehicle \
    sensor_model:=sample_sensor_kit
```

### Message Definition

Message definition files can be discovered from these repositories.

- [autoware\_auto\_msgs](https://github.com/tier4/autoware_auto_msgs)
- [tier4\_autoware\_msgs](https://github.com/tier4/tier4_autoware_msgs)

The path of the message type can be learned from the IDL file
path. For example, the file for `AckermannControlCommand` message from
[autoware\_auto\_control\_msgs](https://github.com/tier4/autoware_auto_msgs/tree/tier4/main/autoware_auto_control_msgs)
is located at:

```
autoware_auto_control_msgs/msg/AckermannControlCommand.idl
```

It can be translated to the Python import path.

```python
from autoware_auto_control_msgs.msg import AckermannControlCommand
```

Let's peek into the `AckermannControlCommand.idl` file shown as follows.

```idl
#include "autoware_auto_control_msgs/msg/AckermannLateralCommand.idl"
#include "autoware_auto_control_msgs/msg/LongitudinalCommand.idl"
#include "builtin_interfaces/msg/Time.idl"

module autoware_auto_control_msgs {
  module msg {
    @verbatim (language="comment", text=
      " Lateral and longitudinal control message for Ackermann-style platforms")
    struct AckermannControlCommand {
      builtin_interfaces::msg::Time stamp;

      autoware_auto_control_msgs::msg::AckermannLateralCommand lateral;
      autoware_auto_control_msgs::msg::LongitudinalCommand longitudinal;
    };
  };
};
```

The layout description can be structured as the tree, showing the the
`AckermannControlCommand` message has three fields.

- module: autoware_auto_control_msgs
  - module: msg
    - struct: AckermannControlCommand
      - field: Time stamp
      - field: AckermannLateralCommand lateral
      - field: LongitudinalCommand longitudinal

### Parameters

TODO(aesop): Configuration, Tricks

- [sensor\_kit\_launch](https://github.com/autowarefoundation/sample_sensor_kit_launch)
- [sample\_vehicle\_launch](https://github.com/autowarefoundation/sample_vehicle_launch)
- [autoware\_individual\_params](https://github.com/autowarefoundation/autoware_individual_params)
