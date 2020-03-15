# Rooster Description
This package contains the URDF description file of the **Ro**o**ster** (**R**ealsense + **O**u**ster**), a compact
handheld device designed to perform visual-inertial and LIDAR navigation algorithms.

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

## Rooster v3:
Note that on Rooster v3, the computer, the camera and the lidar are all in one package. To ease the integration with other 
robots (e.g., Ghost, Husky), the base frame is re-defined to be at the bottom of the device (i.e., the computer case) 
on the intersection between the sagittal and transversal plane, with x-axis forward, y-axis left and z-axis up.
The z-axis coincident to the z-axis Ouster sensor frame, with an offset of 77.250 mm.

**WARNING:** the base frame is NOT coincident with the geometric center of the rectangle made by the bottom screw holes.
There is an offset of 0.450 mm on the left between these two points.

**IMPORTANT:** note also that the Ouster is rotated 45 deg clockwise for cabling reasons.



