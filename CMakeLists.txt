cmake_minimum_required(VERSION 3.10)
project(ob_gins)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS on)
# 默认使用Release模式
if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif ()


project(ic_gvins)


set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)


file(GLOB_RECURSE SOURCE
        src/ob_gins.cc
        src/fileio/fileloader.cc
        src/fileio/filesaver.cc
        src/preintegration/preintegration_base.cc
        src/preintegration/preintegration_normal.cc
        src/preintegration/preintegration_earth.cc
        src/preintegration/preintegration_odo.cc
        src/preintegration/preintegration_earth_odo.cc)

include_directories(${PROJECT_SOURCE_DIR})
add_executable(${PROJECT_NAME} ${SOURCE})

# Eigen3
find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR})

# yaml-cpp
find_package(yaml-cpp REQUIRED)
target_link_libraries(${PROJECT_NAME} ${YAML_CPP_LIBRARIES})

# abseil
set(ABSL_PROPAGATE_CXX_STD true)
add_subdirectory(src/thirdparty/abseil-cpp abseil-cpp)
target_link_libraries(${PROJECT_NAME}
        absl::strings
        absl::str_format
        absl::time)

# Ceres
find_package(Ceres REQUIRED)
target_link_libraries(${PROJECT_NAME} ${CERES_LIBRARIES})
