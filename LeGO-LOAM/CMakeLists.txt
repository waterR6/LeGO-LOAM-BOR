cmake_minimum_required(VERSION 2.8.3)
project(lego_loam_bor)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -O3 -g ")


set(CMAKE_FIND_DEBUG_MODE TRUE)
find_package(catkin REQUIRED COMPONENTS
        tf
        roscpp
        rospy
        image_transport
        rosbag_storage

        pcl_ros
        pcl_conversions

        std_msgs
        sensor_msgs
        geometry_msgs
        nav_msgs
        cloud_msgs
        )

find_package(Boost REQUIRED COMPONENTS thread serialization timer chrono)
find_package(GTSAM REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(PCL REQUIRED)

catkin_package(
        INCLUDE_DIRS include
        CATKIN_DEPENDS cloud_msgs rosbag_storage
        DEPENDS PCL
)

include_directories(
        include
        ${catkin_INCLUDE_DIRS}
        ${PCL_INCLUDE_DIRS}
        ${GTSAM_INCLUDE_DIR}
        ${EIGEN_INCLUDE_DIR}
        ${Boost_INCLUDE_DIR}
)

add_executable(lego_loam_bor
        src/imageProjection.cpp
        src/featureAssociation.cpp
        src/mapOptmization.cpp
        src/transformFusion.cpp
        src/main.cpp)

add_dependencies(lego_loam_bor ${catkin_EXPORTED_TARGETS})
target_link_libraries(lego_loam_bor ${catkin_LIBRARIES} ${PCL_LIBRARIES} gtsam ${Boost_LIBRARIES})

