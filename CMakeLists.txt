# http://ros.org/doc/groovy/api/catkin/html/user_guide/supposed.html
cmake_minimum_required(VERSION 2.8.3)
project(joy)

# Load catkin and all dependencies required for this package
#set(CATKIN_DEPS roscpp diagnostic_updater sensor_msgs)
#set(ROSDEP_DEPS joystick)
#find_package(catkin REQUIRED ${CATKIN_DEPS})
#catkin_package(DEPENDS ${CATKIN_DEPS} ${ROSDEP_DEPS})

#anita
#if(NOT WIN32)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wno-narrowing -Wall -Wdeprecated-declarations")
#  set(CEF_C_COMPILER_FLAGS  -Wno-narrowing)
#endif()

set(ROS_BUILD_TYPE Debug)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)

# Look for <linux/joystick.h>
include(CheckIncludeFiles)
check_include_files(linux/joystick.h HAVE_LINUX_JOYSTICK_H)

if(HAVE_LINUX_JOYSTICK_H)
  include_directories(msg/cpp ${ament_INCLUDE_DIRS})
  add_executable(joy_node src/joy_node.cpp)
  target_link_libraries(joy_node ${ament_LIBRARIES})
else(HAVE_LINUX_JOYSTICK_H)
  message("Warning: no <linux/joystick.h>; won't build joy node")
endif(HAVE_LINUX_JOYSTICK_H)

#catkin_add_nosetests(test/test_joy_msg_migration.py)

# Install targets
install(TARGETS joy_node
  ARCHIVE DESTINATION ${AMENT_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${AMENT_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${AMENT_PACKAGE_BIN_DESTINATION})

install(DIRECTORY migration_rules
  DESTINATION ${AMENT_PACKAGE_SHARE_DESTINATION}
  )
