# Minimum CMake required
cmake_minimum_required(VERSION 3.10)

# Project
project(add_people)

# Find required protobuf package
find_package(Protobuf REQUIRED)

add_executable(${PROJECT_NAME} add_people.cc)
target_link_libraries(${PROJECT_NAME} PUBLIC addressbook ${Protobuf_LIBRARIES})
target_include_directories(${PROJECT_NAME} PUBLIC ${Protobuf_INCLUDE_DIRS})
