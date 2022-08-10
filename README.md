# Frontier Description
This package contains the URDF description file of the Frontier, a compact
handheld device designed to perform visual-inertial and LIDAR navigation algorithms.
It was previously known as **Ro**o**ster** (**R**ealsense + **O**u**ster**) until 2022.

## Dependencies:
 - [realsense2_description](https://github.com/IntelRealSense/realsense-ros/tree/development/realsense2_description) available from the  [Intel's repo](https://github.com/IntelRealSense/realsense-ros) or APT (`ros-noetic-realsense2-description`) 
 - [ddynamic_reconfigure](https://github.com/pal-robotics/ddynamic_reconfigure) this is a dependency of the `realsense2_camera` that should be available through APT (`ros-noetic-ddynamic-reconfigure`).
 - [ouster_description](https://github.com/ori-drs/ouster_example/tree/create-ouster-description/ouster_description) available at the DRS fork of the [ouster_example](https://github.com/ori-drs/ouster_example) repository

## Files
 - `frontier.urdf.xacro`: macro definition of the Frontier device, which you can instantiate on any robot
 - `frontier_standalone.urdf.xacro`: Frontier as a standalone robot. The root link is called `base` and its origin
   is coincident with the `camera_link` frame (see below)
 - `microstrain.urdf.xacro`: macro definition of the Microstrain 3DM-GX4-45 and 3DM-GX4-25 inertial measurement units

## Frontier Macro Parameters
 - `simulation`: if true, it generates all the links for optical frames that are normally advertized by the sensor drivers
 - `parent`: name of the root link of the device (default is: `base`)
 - `origin`: xacro block that connects `parent` to `frontier_mount`
 - `use_ouster`: load the ouster OS lidar instead of the Hesai Pandar
 - `rotate_ouster`: rotate the ouster lidar by 45 degrees (ignored if `use_ouster` is `false`)
 - `raise_lidar`: raises the ouster lidar by 4 mm (some models have standoff, soon to be deprecated)
 - `use_realsense`: load the RealSense D435i instead of the Alphasense Dev Kit

## Standalone Frontier Xacro Arguments:
The following arguments are ported from the macro to the standalone macro:
 - `simulation`
 - `use_ouster`
 - `rotate_ouster`
 - `use_realsense`
 - `raise_lidar`

## Standalone  Frontier URDF Frames:
 - `base` : coincident with the left camera optical frame with x-forward, y-left, z-up convention. The ground truth is expressed in this frame.
 - `frontier_mount`: located at the bottom of the Frontier, with z-axis coincident with the ouster sensor frame.
 - `realsense_parent` : the frame located at the bottom screw hole plate of the RealSense D435i is located here
 - `os_sensor` : conventional base link for the Ouster. 
                  It is at the bottom of the sensor and the top of the Frontier, 0 deg (or 45 deg clockwise rotated on z-axis if `rotate_ouster = true`).
 - `os_lidar` : is looking backward and is in the middle of the sensor. Not used anymore, pointcluds from the ouster are expressed in `os_sensor` since firmeware 2.0
 - `os_imu` : has fixed offset from `os_sensor` retrived from documentation or via interactive probing to the device.
 - `bottom_screws_center`: Convenience frame used for mounting on other robots only.
                           It is at the intersection between the lines connecting the bottom screw holes (not visible on URDF).
 - `alphasense_mount`: frame at the bottom of the alphasense, in the middle of the mounting screws
 - `alphasense_imu`: IMU located inside the Alphasense Dev Core Kit
 - `hesai_base`: located at the bottom of the hesai, concentric with the axis of rotation and with `x`-axis pointing forward
 - `pandar`: sensor origin, with `x`-axis pointing left
 - `cam_front_left`,`cam_front_right`,`cam_side_left`,`cam_side_right`: optical centers of the Alphasense's cameras
                           

## Example
This launches RViz and shows the various frames of the standalone robot:
```
roslaunch frontier_description view_urdf.launch
```
## URDF Generation and Inspection
You can generate the URDF with all the simulated frames by launchng the `xacro` command.  
The resulting URDF can be then inspected with the `urdf_to_graphiz` utility:
```
xacro frontier_standalone.urdf.xacro simulation:=true > frontier.urdf
urdf_to_graphiz frontier.urdf
evince frontier.pdf
```
