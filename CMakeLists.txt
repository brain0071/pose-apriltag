cmake_minimum_required(VERSION 3.0.2)
project(pose_apriltag)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  geometry_msgs
  message_generation
)

# find_package(apriltag realsense)
find_package(realsense2 2.50.0)
if (NOT realsense2_FOUND)
  message(FATAL_ERROR "\n\n Intel RealSense SDK 2.0 is missing, 
  please install it from https://github.com/IntelRealSense/librealsense/releases\n\n")
endif()

add_message_files(
  FILES
  AprilTagDetection.msg
  AprilTagDetectionArray.msg
)

generate_messages(
  DEPENDENCIES
  std_msgs
  geometry_msgs
)


# location share lib, find apriltag.so
link_directories(
  ${catkin_LIB_DIRS}
  /home/dlmux/apriltag/lib
)

catkin_package(
  #INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS 
  message_runtime
  std_msgs
  geometry_msgs
)

# find apriltag.h
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  /home/dlmux/apriltag/include/apriltag
  # /usr/include/eigen3
)
add_executable(pose_apriltag src/pose_apriltag.cpp)
target_link_libraries(pose_apriltag ${catkin_LIBRARIES} apriltag ${realsense2_LIBRARY})








