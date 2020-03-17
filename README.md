# Rooster Description
This package contains the URDF description file of the **Ro**o**ster** (**R**ealsense + **O**u**ster**), a compact
handheld device designed to perform visual-inertial and LIDAR navigation algorithms.

## Dependencies:
 - [realsense2_description](https://github.com/ori-drs/realsense/tree/development-fixes) fork of the [official Intel's repo](https://github.com/IntelRealSense/realsense-ros) at its latest development release (2.2.13) with some additional issue fixes.
 - [ddynamic_reconfigure](https://github.com/pal-robotics/ddynamic_reconfigure) this is a dependency of the `realsense2_camera` that should be available through APT (`ros-melodic-ddynamic-reconfigure`), but if you experience compilation issues, clone this fork at `kinetic-devel` branch.

## Files
 - `rooster.urdf.xacro`: macro definition of the Rooster device, which you can instantiate on any robot
 - `rooster_standalone.urdf.xacro`: Rooster as a standalone robot. The root link is called `base` and its origin
   is coincident with the `camera_link` frame (see below)

## Macro Parameters
 - `simulation`: if true, it generates all the links for optical frames that are normally advertized by the sensor drivers
 - `parent`: name of the root link of the device (default is: `base`)
 - `origin`: xacro block that connects `parent` to `base_mount`

## Standalone Xacro Arguments:
 - `simulation` : same as the macro parameter

## Frames:
 - `base` : coincident with the left camera optical frame with x-forward, y-left, z-up convention. The ground truth is expressed in this frame.
 - `base_mount`: located at the bottom of the Rooster, with z-axis coincident with the ouster sensor frame.
 - `realsense_parent` : the frame located at the bottom screw hole plate of the RealSense D435i is located here
 - `os1_sensor` : conventional base link for the Ouster. 
                  It is at the bottom of the sensor and the top of the Rooster, 45 deg clockwise rotate on z-axis due to cabling.
 - `os1_lidar` : is looking backward and is in the middle of the sensor (as per official documentation)
 - `os1_imu` : has fixed offset from `os1_sensor` retrived from documentation or via interactive probing to the device.
 - `bottom_screws_center`: Convenience frame used for mounting on other robots only.
                           It is at the intersection between the lines connecting the bottom screw holes (not visible on URDF). 
                           

## Example
This launches RViz and shows the various frames of the standalone robot:
```
roslaunch rooster_description view_urdf.launch
```


