cmake_minimum_required(VERSION 2.8.3)
project(ps3joy)

find_package(PkgConfig REQUIRED)
pkg_search_module(LIBUSB REQUIRED libusb)

find_package(catkin REQUIRED COMPONENTS
  diagnostic_msgs
  rosgraph
  rospy
  sensor_msgs
)
catkin_package()

include_directories(${catkin_INCLUDE_DIRS})

if(LIBUSB_FOUND)
  include_directories(SYSTEM ${LIBUSB_INCLUDE_DIRS})
  link_directories(${LIBUSB_LIBRARY_DIRS})

  add_executable(sixpair src/sixpair.c)
  target_link_libraries(sixpair -lusb ${catkin_LIBRARIES})

  install(TARGETS sixpair
    ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
  )
else()
  message( FATAL_ERROR "Failed to find libusb" )
endif(LIBUSB_FOUND)

# Install targets
install(DIRECTORY launch 
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

install(FILES diagnostics.yaml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

install(PROGRAMS scripts/ps3joy.py scripts/ps3joy_node.py scripts/ps3joysim.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
