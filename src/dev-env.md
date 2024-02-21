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

## The Autoware Complex

Autoware itself is a large ROS project. The packaging and execution
follow ROS convention. Autoware provides various kinds of components
to complete an autonomous vehicle system, including perception,
planning and more parts. It's recommended to explore the node
[diagram](https://autowarefoundation.github.io/autoware-documentation/main/design/autoware-architecture/node-diagram/overall-node-diagram-autoware-universe.drawio.svg)
to explore the packages and their relations.


- Message type definitions
  - [autoware\_auto\_msgs](https://github.com/tier4/autoware_auto_msgs)
  - [tier4\_autoware\_msgs](https://github.com/tier4/tier4_autoware_msgs)
- Launch script packages to start a component and the whole system.
  - [autoware\_launch](https://github.com/autowarefoundation/autoware_launch)
    to launch the whole system.
  - [sample\_sensor\_kit\_launch](https://github.com/autowarefoundation/sample_sensor_kit_launch)
    to launch sensor driving nodes.
  - [sample\_vehicle\_launch](https://github.com/autowarefoundation/sample_vehicle_launch)
    to launch motor controlling nodes
- Packages for vehicle sensing, control, etc
  - See the section below the learn the Autoware project layout.


## Understanding Autoware Launch System

The way to run Autoware depends on the execution scenarios, which are
written as respective launch files. The files can be found in the
[autoware\_launch](https://github.com/autowarefoundation/autoware_launch/tree/main/autoware_launch/launch)
package.  The launch file `autoware.launch.xml` is entry file to
launch the whole system. To launch this file, these arguments
`map_path`, `vehicle_model`, and `sensor_model` must be assigned.

```bash
ros2 launch autoware_launch autoware.launch.xml \
    map_path:=$HOME/autoware_map/sample-map-planning \
    vehicle_model:=sample_vehicle \
    sensor_model:=sample_sensor_kit 
```

TODO(aesop): Configuration, Tricks


## Autoware Source Code Layout

Autoware source code consists of many GitHub repositories. The
top-level repository is
[autowarefoundation/autoware](https://github.com/autowarefoundation/autoware),
which does not include any source code. It has a `autoware.repos` file
listing dependent repos. Users have to run `vcs import <
autoware.repos` to download these repositories.

Note that `vcs` does not provide fine-grained version control. Hence,
downloaded repos do not pin to specific Git commits, but tracks to
update-to-date master branch. It makes troubles sometimes because repos
change frequently.

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
