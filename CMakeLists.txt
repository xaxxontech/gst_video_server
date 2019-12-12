cmake_minimum_required(VERSION 2.8.3)
project(gst_video_server)

## Find catkin macros and libraries
find_package(catkin REQUIRED COMPONENTS
  nodelet
  image_transport
  roscpp
  sensor_msgs
  std_msgs
)

## GStreamer 1.0 package required
find_package(PkgConfig)


pkg_check_modules(gst gstreamer-1.0)
pkg_check_modules(gstbase gstreamer-base-1.0)
pkg_check_modules(gstapp gstreamer-app-1.0)


message(STATUS "GST-VIDEO-SERVER inc: ${gstreamer_INCLUDE_DIRS}")
message(STATUS "GST-VIDEO-SERVER lib: ${gstreamer_LIBRARIES}")


###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
#  INCLUDE_DIRS include
  # LIBRARIES gstvideoserver
  # CATKIN_DEPENDS nodelet image_transport roscpp sensor_msgs std_msgs
  # DEPENDS libgstreamer1.0-dev
)

###########
## Build ##
###########

include_directories(
  ${catkin_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
  ${gst_INCLUDE_DIRS}
  ${gstapp_INCLUDE_DIRS}
  ${gstbase_INCLUDE_DIRS}
)

# added new
link_directories(
	${gstbase_LIBRARY_DIRS}
	${gstapp_LIBRARY_DIRS}
	${gst_LIBRARY_DIRS}
)

add_library(gstvideoserver
  src/server_nodelet.cpp
)
target_link_libraries(gstvideoserver
   ${catkin_LIBRARIES}
	${gstapp_LIBRARIES}
	${gstbase_LIBRARIES}
	${gst_LIBRARIES}
)

add_executable(server_node
  src/node.cpp
)
target_link_libraries(server_node
   ${catkin_LIBRARIES}
	${gstapp_LIBRARIES}
	${gstbase_LIBRARIES}
	${gst_LIBRARIES}
)

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executables and/or libraries for installation
install(TARGETS gstvideoserver server_node
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

#install(DIRECTORY include/${PROJECT_NAME}/
#  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#  FILES_MATCHING PATTERN "*.h"
#)

## Mark other files for installation (e.g. launch and bag files, etc.)
install(FILES
  nodelet_plugins.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

#install(DIRECTORY launch/
#  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
#)

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_rtsp_streamer.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)

# vim:set ts=2 sw=2 et :
