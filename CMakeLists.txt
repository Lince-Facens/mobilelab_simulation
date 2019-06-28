cmake_minimum_required(VERSION 2.8.3)
project(mobilelab_simulation)

set (MOBILELAB_MAJOR_VERSION 0)
set (MOBILELAB_MINOR_VERSION 1)
set (MOBILELAB_PATCH_VERSION 0)
set (MOBILELAB_VERSION_FULL ${MOBILELAB_MAJOR_VERSION}.${MOBILELAB_MINOR_VERSION}.${MOBILELAB_PATCH_VERSION})

find_package(catkin REQUIRED COMPONENTS
  gazebo_ros
  mobilelab_msgs
)

find_package(gazebo 9 REQUIRED)
find_package(ignition-msgs1 REQUIRED)

catkin_package(
 # INCLUDE_DIRS include
 # LIBRARIES gazebo_radar_plugin
 CATKIN_DEPENDS gazebo_ros mobilelab_msgs
#  DEPENDS system_lib
)

include_directories(include ${catkin_INCLUDE_DIRS} ${GAZEBO_INCLUDE_DIRS} ${SDFormat_INCLUDE_DIRS})

add_library(PriusHybridPlugin SHARED plugins/PriusHybridPlugin.cc)
target_link_libraries(PriusHybridPlugin ${GAZEBO_LIBRARIES} ${IGNITION-MSGS_LIBRARIES})
add_dependencies(PriusHybridPlugin ${catkin_EXPORTED_TARGETS})


catkin_install_python(PROGRAMS nodes/joystick_translator
                      DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

install(TARGETS PriusHybridPlugin
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY models DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
install(DIRECTORY worlds DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
