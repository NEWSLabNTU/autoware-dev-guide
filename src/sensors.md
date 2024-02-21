# Sensors

Sensor packages are usually maintained by vendors. It usually ships a
vendor-specific driver and a ROS package depending on that driver. For
example, the system has to install `blickfeld-scanner-lib` to allow
the ROS package to scan data from Blickfeld Cube1 LiDARs.

There are chances that the ROS package could be out-of-date, the driver
is incompatible with the box, or the vendor does not provide ROS
implementation. For example, ZED X shipped a closed-source driver
running on old Ubuntu 20.04, which OS is incompatible with Autoware.
Blickfeld ships an old ROS package not satisfying Autoware's need. Ask
the vendor for support to resolve these issues. Good luck with your
cherished sensor.

## LiDARs and Radars

It's common that the LiDAR sensor connects to the box via an Ethernet
cable, transmitting UDP packets in vendor-specific format. The ROS
node or the driver listens and converts them into point cloud frames
in `sensor_msgs/msg/PointCloud2` message type.

These steps must be done providing that LiDARs are Ethernet connected.

- The box and the LiDAR reside in the local network sharing a common IP
  prefix. You may assign a static IP to the box.

  - e.g. The Velodyne VLP32-C has the address 192.168.10.1. The box
    should be addressed at 192.168.10.x other than 192.168.10.1.

- Some boxes, such as AGX Orin, have only one Ethernet port. Use
  USB-to-Ethernet adapter would be your friend.

## Cameras

TODO(Jerry)
