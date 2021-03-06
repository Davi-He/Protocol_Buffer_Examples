# Minimum CMake required
cmake_minimum_required(VERSION 3.10)

# Project
project(proto-generations)

# Find required protobuf package
find_package(Protobuf REQUIRED)

# Print version messages
if(Protobuf_FOUND)
    message(STATUS "Using Protocol Buffers ${Protobuf_VERSION}")
endif()

# Get the filenames of all the proto files.
file(GLOB PROTO_FILES ${CMAKE_CURRENT_SOURCE_DIR}/*.proto)

# Generate header and source files for all the proto files
foreach(proto ${PROTO_FILES})
    # Get filename without extension
    get_filename_component(PROTO_NAME_WE ${proto} NAME_WE)
    get_filename_component(PROTO_NAME ${proto} NAME)
    message(STATUS "Generating Protobuf Code for ${PROTO_NAME} : ${proto}")
    protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS ${proto})
    add_library(${PROTO_NAME_WE} STATIC ${PROTO_SRCS} ${PROTO_HDRS})
    target_include_directories(${PROTO_NAME_WE} PUBLIC ${Protobuf_INCLUDE_DIRS})

    protobuf_generate_python(PROTO_PY ${proto})
    add_custom_target(myTarget ALL DEPENDS ${PROTO_PY})
endforeach()

