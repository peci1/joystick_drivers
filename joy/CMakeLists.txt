cmake_minimum_required(VERSION 2.8.3)
project(joy)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  diagnostic_updater
  sensor_msgs
)
catkin_package()

# Look for <linux/joystick.h>
include(CheckIncludeFiles)
check_include_files(linux/joystick.h HAVE_LINUX_JOYSTICK_H)

if(HAVE_LINUX_JOYSTICK_H)
  include_directories(${catkin_INCLUDE_DIRS})
  add_executable(joy_node src/joy_node.cpp)
  target_link_libraries(joy_node ${catkin_LIBRARIES})

  install(TARGETS joy_node
    ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
  )
else(HAVE_LINUX_JOYSTICK_H)
  message("Warning: no <linux/joystick.h>; won't build joy node")
endif(HAVE_LINUX_JOYSTICK_H)

#catkin_add_nosetests(test/test_joy_msg_migration.py)

install(DIRECTORY migration_rules scripts config launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
  USE_SOURCE_PERMISSIONS
)
