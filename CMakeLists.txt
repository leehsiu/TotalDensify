cmake_minimum_required (VERSION 2.8)
project(totaldensify_c_ops)

SET(CMAKE_C_COMPILER ${CMAKE_CXX_COMPILER})
SET(CMAKE_CXX_STANDARD 11)
# SET(CMAKE_SKIP_BUILD_RPATH  FALSE)
# SET(CMAKE_BUILD_WITH_INSTALL_RPATH FALSE)
# SET(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)
# SET(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib")

find_package( PkgConfig )
pkg_check_modules( EIGEN3 REQUIRED eigen3 )
find_package(OpenMP REQUIRED)
set(CUDA_USE_STATIC_CUDA_RUNTIME OFF)
set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wpedantic -Wall -Wextra -std=c++11")
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")

find_package( OpenCV  REQUIRED )
include_directories(${EIGEN3_INCLUDE_DIRS})
find_package(Ceres REQUIRED COMPONENTS SuiteSparse)
include_directories(${CERES_INCLUDE_DIRS})
#set(IGL_INCLUDE_DIRS "/home/xiul/Tools/libigl/include/")
#server version

include_directories("./fullycapture/cpp/include")
find_package(OpenGL REQUIRED)
find_package(GLUT REQUIRED)
find_package(GLEW REQUIRED)
include_directories( ${OPENGL_INCLUDE_DIRS}  ${GLUT_INCLUDE_DIRS} ${GLEW_INCLUDE_DIRS})

# list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules")
# find_package(GFlags REQUIRED)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules")
find_package(GFlags REQUIRED)
# file(GLOB JSON_SOURCES "src/lib_json/*.cpp")
file(GLOB SOURCES "./fullycapture/cpp/src/*.cpp")
add_library(totalCops SHARED fullycapture/cpp/src/cwrapper.cpp ${SOURCES} ${JSON_SOURCES})
target_link_libraries( totalCops ${OpenCV_LIBS} )
target_link_libraries( totalCops ${CERES_LIBRARIES})
target_link_libraries( totalCops ${OPENGL_LIBRARIES} ${GLUT_LIBRARY} ${GLEW_LIBRARY})
