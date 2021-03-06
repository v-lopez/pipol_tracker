cmake_minimum_required(VERSION 2.8.3)
project(pipol_tracker_pkg)

## Find catkin macros and libraries
find_package( catkin REQUIRED COMPONENTS 
      roscpp
      rospy
      std_msgs
      geometry_msgs
      nav_msgs
      image_transport
      cv_bridge
      message_generation
      dynamic_reconfigure 
      pal_vision_msgs
      pal_detection_msgs      
)

## System dependencies are found with CMake's conventions
# find_package( pipol_tracker REQUIRED )
find_package(OpenCV REQUIRED)

#SRCS files. Pipol tracker library
SET(SRCS_PT_LIB 
      src/pipol_tracker_lib/personParticle.cpp
      src/pipol_tracker_lib/personParticleFilter.cpp
      src/pipol_tracker_lib/personTarget.cpp
      src/pipol_tracker_lib/peopleTracker.cpp
      src/pipol_tracker_lib/geometry/point.cpp
      src/pipol_tracker_lib/geometry/line.cpp
      src/pipol_tracker_lib/geometry/point3d.cpp      
      src/pipol_tracker_lib/geometry/point3dCov.cpp      
      src/pipol_tracker_lib/observations/timeStamp.cpp
      src/pipol_tracker_lib/observations/basicObservation.cpp
      src/pipol_tracker_lib/observations/odometryObservation.cpp
      src/pipol_tracker_lib/observations/point3dObservation.cpp
      src/pipol_tracker_lib/observations/bodyObservation.cpp
      src/pipol_tracker_lib/observations/faceObservation.cpp
      src/pipol_tracker_lib/random/simpleRnd.cpp
)


#SRCS files. ROS node
SET(SRCS_ROS_NODE 
      src/pipol_tracker_node.cpp
      src/pipol_tracker_main.cpp
)

## Add messages to be generated
add_message_files(
      FILES 
      person.msg
      personArray.msg
)

## Generate added messages and services
generate_messages(
      DEPENDENCIES 
      std_msgs 
      geometry_msgs
      pal_vision_msgs
      pal_detection_msgs
)

#dynamic reconfigure cfg file
generate_dynamic_reconfigure_options(cfg/pipol_tracker_config.cfg)

## The catkin_package macro generates cmake config files for your package
catkin_package (
      CATKIN_DEPENDS 
            roscpp 
            rospy 
            std_msgs 
            geometry_msgs
            nav_msgs
            image_transport 
            cv_bridge 
            message_runtime 
            dynamic_reconfigure
            pal_vision_msgs
            pal_detection_msgs
#       DEPENDS 
#             pipol_tracker_lib 
)

## Specify additional locations of header files
# include_directories($ENV{EIGEN3_INCLUDE_DIR} ${pipol_tracker_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})
include_directories(src/pipol_tracker_lib/ ${OpenCV_INCLUDE_DIR} $ENV{EIGEN3_INCLUDE_DIR} ${catkin_INCLUDE_DIRS})

## Declare a cpp executable
add_executable(pipol_tracker_node ${SRCS_ROS_NODE} ${SRCS_PT_LIB})
add_dependencies(pipol_tracker_node ${PROJECT_NAME}_EXPORTED_TARGETS)
# target_link_libraries(pipol_tracker_node ${pipol_tracker_LIBRARIES} ${catkin_LIBRARIES})
target_link_libraries(pipol_tracker_node ${OpenCV_LIBS} ${catkin_LIBRARIES})
