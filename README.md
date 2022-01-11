# Frontier Description
This package contains the URDF description file of the Frontier, a compact
handheld device designed to perform visual-inertial and LIDAR navigation algorithms.
It was previously known as **Ro**o**ster** (**R**ealsense + **O**u**ster**) until 2022.

## Dependencies:
 - [realsense2_description](https://github.com/IntelRealSense/realsense-ros/tree/development/realsense2_description) available from the  [Intel's repo](https://github.com/IntelRealSense/realsense-ros) or APT (`ros-melodic-realsense2-description`) 
 - [ddynamic_reconfigure](https://github.com/pal-robotics/ddynamic_reconfigure) this is a dependency of the `realsense2_camera` that should be available through APT (`ros-melodic-ddynamic-reconfigure`).
 - [ouster_description](https://github.com/ori-drs/ouster_example/tree/create-ouster-description/ouster_description) available at the DRS fork of the [ouster_example](https://github.com/ori-drs/ouster_example) repository

## Files
 - `frontier.urdf.xacro`: macro definition of the Frontier device, which you can instantiate on any robot
 - `frontier_standalone.urdf.xacro`: Frontier as a standalone robot. The root link is called `base` and its origin
   is coincident with the `camera_link` frame (see below)
 - `ouster.urdf.xacro`: macro definition of the Ouster OS-1 LIDAR sensor

## Frontier Macro Parameters
 - `simulation`: if true, it generates all the links for optical frames that are normally advertized by the sensor drivers
 - `parent`: name of the root link of the device (default is: `base`)
 - `origin`: xacro block that connects `parent` to `frontier_mount`
 - `ground_camera`: if true, it adds the ground facing camera and the relative shim to support it

## Standalone Frontier Xacro Arguments:
 - `simulation` : same as the macro parameter
 - `ground_camera`: same as the macro parameter

## Standalone  Frontier URDF Frames:
 - `base` : coincident with the left camera optical frame with x-forward, y-left, z-up convention. The ground truth is expressed in this frame.
 - `frontier_mount`: located at the bottom of the Frontier, with z-axis coincident with the ouster sensor frame.
 - `realsense_parent` : the frame located at the bottom screw hole plate of the RealSense D435i is located here
 - `os1_sensor` : conventional base link for the Ouster. 
                  It is at the bottom of the sensor and the top of the Frontier, 45 deg clockwise rotate on z-axis due to cabling.
 - `os1_lidar` : is looking backward and is in the middle of the sensor (as per official documentation)
 - `os1_imu` : has fixed offset from `os1_sensor` retrived from documentation or via interactive probing to the device.
 - `bottom_screws_center`: Convenience frame used for mounting on other robots only.
                           It is at the intersection between the lines connecting the bottom screw holes (not visible on URDF). 
 - `ground_camera_shim`: concentric with top screw of the 40 deg down shim, on the surface of the NUC cover
                           

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
