cmake_minimum_required(VERSION 3.0.2)
project(frontier_description)

find_package(catkin REQUIRED COMPONENTS
  realsense2_description  # To throw a failure if the realsense2_description is not installed
  alphasense_description
  ouster_description
  hesai_description
  xacro
)

catkin_package(CATKIN_DEPENDS xacro)

set(xacro_files
  ${CMAKE_CURRENT_SOURCE_DIR}/urdf/frontier_standalone.urdf.xacro
)

xacro_add_files(${xacro_files} INSTALL DESTINATION urdf)

install(DIRECTORY config launch urdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
