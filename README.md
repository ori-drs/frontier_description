# Rooster Description

## Dependencies:
 - [realsense2_description](https://github.com/IntelRealSense/realsense-ros/tree/development/realsense2_description)
   note that this is available as `ros-melodic-realsense2-description` in mainstream ROS

## Parameters:
 - `simulation` : when active, it creates the extra frames that are normaly advertized by the device drivers through TF

## Frames:
 - `base` : is coincident with the ouster sensor frame: x-forward, y-left, z-up, at the center bottom plane of the Ouster
 - `realsense_parent` : the frame located at the bottom screw hole plate of the RealSense D435i is located here
 - `os1_sensor` : conventional base link for the Ouster, located at bottom plate and centered on the device
 - `os1_lidar` : is looking backward and is in the middle of the device (as per official documentation)
 - `os1_imu` : has fixed offset from `os1_sensor`

## Example
This launches RViz and shows the various frames
```
roslaunch rooster_description view_urdf.launch
```
