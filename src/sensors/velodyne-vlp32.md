# Velodyne VLP-32C

TODO(Allan&子騰)

Sensor’s configuration can be modified in file
`autoware/src/sensor_kit/sample_sensor_kit_launch/common_sensor_launch/launch/velodyne_VLS128.launch.xml`,
since we choose to other sensor_kit setting rather than create one for
our usage.

```xml
<arg name="model" default="VLP32"/>

<!-- omit... -->

<arg name="sensor_ip" default="192.168.10.10"/>
```

To correct lidar’s relative position and direction, compare with
vehicle, you must modified the configuration in file
`autoware/src/param/autoware_individual_params/individual_params/config/default/sample_sensor_kit/sensors_calibration.yaml`.

(similar file named sensors_calibration.yaml may appear at multiple
places, you should be aware of the file path)

```bash
base_link:
  sensor_kit_base_link:
    x: 0.9
    y: 0.0
    z: 2.0
    roll: -0.001
    pitch: 0.015
    yaw: -0.0364
  velodyne_rear_base_link:
    x: -0.358
    y: 0.0
    z: 1.631
    roll: -0.02
    pitch: 0.7281317
    yaw: 3.141592
```
