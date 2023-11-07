cmake_minimum_required(VERSION 3.0.2)
project(pose-apriltag)


find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
)

# find_package(apriltag realsense)
find_package(realsense2 2.50.0)
if (NOT realsense2_FOUND)
  message(FATAL_ERROR "\n\n Intel RealSense SDK 2.0 is missing, 
  please install it from https://github.com/IntelRealSense/librealsense/releases\n\n")
endif()

# location share lib, find apriltag.so
link_directories(
  ${catkin_LIB_DIRS}
  /home/naodai/apriltag/lib
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
)

# find apriltag.h
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  /home/naodai/apriltag/include/apriltag
)

add_executable(pose-apriltag src/pose-apriltag.cpp)
target_link_libraries(pose-apriltag ${catkin_LIBRARIES} apriltag ${realsense2_LIBRARY})








