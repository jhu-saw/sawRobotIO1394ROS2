#
# (C) Copyright 2023 Johns Hopkins University (JHU), All Rights Reserved.
#
# --- begin cisst license - do not edit ---
#
# This software is provided "as is" under an open source license, with
# no warranty.  The complete license can be found in license.txt and
# http://www.cisst.org/cisst/license.txt.
#
# --- end cisst license ---

cmake_minimum_required (VERSION 3.10)
project (robot_io VERSION 2.2.1)

find_package (ament_cmake REQUIRED)
find_package (rclcpp REQUIRED)
find_package (std_msgs REQUIRED)
find_package (std_srvs REQUIRED)
find_package (geometry_msgs REQUIRED)
find_package (sensor_msgs REQUIRED)
find_package (diagnostic_msgs REQUIRED)
find_package (cisst_msgs REQUIRED)
find_package (tf2_ros REQUIRED)
find_package (tf2_msgs REQUIRED)
find_package (crtk_msgs REQUIRED)
find_package (cisst_ros2_bridge REQUIRED)
find_package (cisst_ros2_crtk REQUIRED)

# find cisst and make sure the required libraries have been compiled
set (REQUIRED_CISST_LIBRARIES
     cisstCommon
     cisstCommonQt
     cisstVector
     cisstVectorQt
     cisstOSAbstraction
     cisstMultiTask
     cisstMultiTaskQt
     cisstParameterTypes
     cisstParameterTypesQt
     cisstQt
     )

find_package (cisst 1.2.1 REQUIRED ${REQUIRED_CISST_LIBRARIES})

if (cisst_FOUND_AS_REQUIRED)

  include (${CISST_USE_FILE})

  find_package (sawRobotIO1394 2.2.1 REQUIRED)

  include_directories (
    ${robot_io_SOURCE_DIR}
    ${CATKIN_DEVEL_PREFIX}/include
    ${sawRobotIO1394_INCLUDE_DIR}
    )

  link_directories (
    ${sawRobotIO1394_LIBRARY_DIR}
    )

  # library
  add_library (ros_crtk_robot_io_bridge
    mts_ros_crtk_robot_io_bridge.h
    mts_ros_crtk_robot_io_bridge.cpp
    )
  target_link_libraries (
    ros_crtk_robot_io_bridge
    ${sawRobotIO1394_LIBRARIES}
    ${catkin_LIBRARIES}
    )
  cisst_target_link_libraries (ros_crtk_robot_io_bridge ${REQUIRED_CISST_LIBRARIES})
  ament_target_dependencies (
    ros_crtk_robot_io_bridge
    rclcpp
    std_msgs
    std_srvs
    geometry_msgs
    sensor_msgs
    diagnostic_msgs
    cisst_msgs
    tf2_ros
    crtk_msgs
    cisst_ros2_bridge
    cisst_ros2_crtk
    )

  # executable
  add_executable (robot_io robot_io_console.cpp)
  target_link_libraries (
    robot_io
    ${sawRobotIO1394_LIBRARIES}
    ${catkin_LIBRARIES}
    )
  cisst_target_link_libraries (robot_io ${REQUIRED_CISST_LIBRARIES})
  target_link_libraries (robot_io ros_crtk_robot_io_bridge)
  ament_target_dependencies (
    robot_io
    rclcpp
    std_msgs
    geometry_msgs
    sensor_msgs
    diagnostic_msgs
    cisst_msgs
    tf2_ros
    crtk_msgs
    cisst_ros2_bridge
    cisst_ros2_crtk
    )

  install (TARGETS ros_crtk_robot_io_bridge robot_io
    LIBRARY DESTINATION lib/${PROJECT_NAME}
    RUNTIME DESTINATION lib/${PROJECT_NAME}
    LIBRARY DESTINATION lib
    INCLUDES DESTINATION include
    )

  ament_package ()

endif (cisst_FOUND_AS_REQUIRED)
