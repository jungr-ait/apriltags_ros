cmake_minimum_required(VERSION 2.8.3)
project(apriltags)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS cmake_modules
)

find_package(Eigen REQUIRED)
find_package(OpenCV REQUIRED)
find_package(OpenMP REQUIRED)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES apriltags
  CATKIN_DEPENDS
  DEPENDS Eigen OpenCV OpenMP
)

include_directories(include include/AprilTags)
include_directories(
  ${Eigen_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
)

file(GLOB_RECURSE include_files ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h*)
file(GLOB_RECURSE src_files ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c*)

add_library(apriltags
  ${include_files}
  ${src_files}
)

if(OPENMP_FOUND)
 set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
 set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
 set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

target_link_libraries(apriltags
  ${Eigen_LIBRARIES}
  ${OpenCV_LIBRARIES}
  ${catkin_LIBRARIES}
  -fopenmp
)


#############
## Install ##
#############

## Mark executables and/or libraries for installation
install(TARGETS apriltags
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

## Mark cpp header files for installation
install(DIRECTORY include/
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
)
