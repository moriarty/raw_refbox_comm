find_package(Protobuf REQUIRED)
include_directories(${PROTOBUF_INCLUDE_DIRS})

file(GLOB RAW_REFBOX_MSGS_PROTOS ${CMAKE_CURRENT_SOURCE_DIR}/../../proto/*.proto)
protobuf_generate_cpp(REFBOX_PROTO_SRCS REFBOX_PROTO_HDRS ${RAW_REFBOX_MSGS_PROTOS})
add_library(${DISAMBIGUATION}raw_refbox_msgs ${REFBOX_PROTO_SRCS})
target_link_libraries(${DISAMBIGUATION}raw_refbox_msgs ${PROTOBUF_LIBRARIES})

set(RAW_REFBOX_MSGS_HPP "${CMAKE_CURRENT_BINARY_DIR}/../raw_refbox_msgs.h")
file(REMOVE ${RAW_REFBOX_MSGS_HPP})
foreach(PB_H ${REFBOX_PROTO_HDRS})
   get_filename_component(FILE_PB_H ${PB_H} NAME)
   file(APPEND ${RAW_REFBOX_MSGS_HPP} "#include \"raw_refbox_msgs/${FILE_PB_H}\"\n")
endforeach()

file(
    COPY ${REFBOX_PROTO_HDRS} DESTINATION ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_INCLUDE_DESTINATION}/${PROJECT_NAME}
)

include_directories(${CMAKE_CURRENT_BINARY_DIR}/../)

set(INCLUDE_DIRS ${INCLUDE_DIRS} ${CMAKE_CURRENT_BINARY_DIR}/../ PARENT_SCOPE)
