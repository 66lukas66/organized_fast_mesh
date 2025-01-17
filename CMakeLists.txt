cmake_minimum_required(VERSION 2.4.6)

project(organized_fast_mesh)

find_package(catkin REQUIRED COMPONENTS
        mesh_msgs
        mesh_msgs_conversions
        roscpp
        rospy
        std_msgs
        sensor_msgs
        message_generation

        )

find_package(LVR2 REQUIRED)



add_definitions(${LVR2_DEFINITIONS})

include_directories(
        include
        ${catkin_INCLUDE_DIRS}

        ${LVR2_INCLUDE_DIRS}
)

link_directories(
        ${catkin_LIBRARY_DIRS}

        ${LVR2_LIBRARY_DIRS}
)

add_service_files(
        FILES
        OrganizedFastMeshSrv.srv

)


generate_messages(
        DEPENDENCIES
        std_msgs
        sensor_msgs
        mesh_msgs
        geometry_msgs
)

catkin_package(
        CATKIN_DEPENDS mesh_msgs mesh_msgs_conversions roscpp std_msgs sensor_msgs message_runtime
        INCLUDE_DIRS include
        LIBRARIES organized_fast_mesh
        DEPENDS LVR2
)
add_library(organized_fast_mesh src/organized_fast_mesh.cpp src/organized_fast_mesh_generator.cpp)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})
add_executable(organized_fast_mesh_node src/organized_fast_mesh.cpp src/organized_fast_mesh_generator.cpp)

target_link_libraries(organized_fast_mesh_node
        ${catkin_LIBRARIES}
        ${LVR2_LIBRARIES}
        )



add_dependencies(organized_fast_mesh_node ${catkin_EXPORTED_TARGETS} ${PROJECT_NAME}_generate_messages_cpp)
add_dependencies(organized_fast_mesh ${catkin_EXPORTED_TARGETS} ${PROJECT_NAME}_generate_messages_cpp)


install(
        DIRECTORY include/${PROJECT_NAME}/ DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})

install(
        TARGETS organized_fast_mesh organized_fast_mesh_node
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

