cmake_minimum_required(VERSION 2.8.3)
project(spacenav_node)

find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  roscpp
  sensor_msgs
)
catkin_package()

include_directories(${catkin_INCLUDE_DIRS})
add_executable(spacenav_node src/spacenav_node.cpp)
target_link_libraries(spacenav_node spnav X11 ${catkin_LIBRARIES})

# Install targets
install(TARGETS spacenav_node
 ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
 LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
 RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY launch 
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
