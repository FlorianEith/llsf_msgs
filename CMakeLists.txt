cmake_minimum_required(VERSION 2.8.7)
project(llsf_msgs)
find_package(catkin REQUIRED)

find_package(Protobuf REQUIRED)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

include_directories(${PROTOBUF_INCLUDE_DIRS})

set(REFBOX_ROOT_DIR /home/faps/llsf-refbox)
set(LLSF_MSGS_DIR "${REFBOX_ROOT_DIR}/src/msgs")

IF(EXISTS "${REFBOX_ROOT_DIR}" AND IS_DIRECTORY "${REFBOX_ROOT_DIR}")
  MESSAGE(STATUS "refbox directory found")
ELSE()
  MESSAGE(FATAL_ERROR "\n"
     "Refbox directory " ${REFBOX_ROOT_DIR} " doesn't exist.\n"
     "Check if the prerequesites have been initialized.\n"
     "Please see README in \"llsf_msgs\"\n"
  )
ENDIF(EXISTS "${REFBOX_ROOT_DIR}" AND IS_DIRECTORY "${REFBOX_ROOT_DIR}")



IF(EXISTS "${LLSF_MSGS_DIR}" AND IS_DIRECTORY "${LLSF_MSGS_DIR}")
  MESSAGE(STATUS "protobuf msgs directory found")
ELSE()
  MESSAGE(FATAL_ERROR "\n"
     "Protobuf msgs " ${LOGISTIGS_MSGS_DIR} " directory doesn't exist.\n"
     "Check if the submodule has been initialized.\n"
     "Please see README in \"llsf_msgs\"\n"
  )
ENDIF(EXISTS "${LLSF_MSGS_DIR}" AND IS_DIRECTORY "${LLSF_MSGS_DIR}")

file(GLOB REFBOX_PROTOS ${REFBOX_ROOT_DIR}/src/msgs/*.proto)

#MESSAGE(STATUS "Refbox Protos: "${REFBOX_PROTOS})

protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS ${REFBOX_PROTOS})



#MESSAGE(STATUS "Refbox Protos Src data: "${REFBOX_SRCS})

add_custom_target(copy ALL
  DEPENDS ${PROTO_HDRS}
)


add_library(llsf_msgs SHARED ${PROTO_SRCS})
target_link_libraries(llsf_msgs ${PROTOBUF_LIBRARIES})


catkin_package(
  LIBRARIES
    llsf_msgs
)



file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_INCLUDE_DESTINATION}/${PROJECT_NAME} )

foreach(File ${PROTO_HDRS})
  add_custom_command(TARGET copy POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different ${File} ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_INCLUDE_DESTINATION}/${PROJECT_NAME} 
  )
endforeach()

install(FILES ${PROTO_HDRS}
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

install(TARGETS llsf_msgs
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

## else: using cmake
#else()
#  install(
#    TARGETS rcll_pb_msgs
#    LIBRARY DESTINATION lib
#  )
#  install(
#    FILES ${PROTO_HDRS}
#    DESTINATION include/${PROJECT_NAME}
#  )
#
#endif(DEFINED _CATKIN_ALL_INCLUDED_)

