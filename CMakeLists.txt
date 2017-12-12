cmake_minimum_required(VERSION 3.5)
project(robocamcal)

# check if I am a top-level project
if(CMAKE_SOURCE_DIR STREQUAL PROJECT_SOURCE_DIR)
    # using c++11
    set(CMAKE_CXX_STANDARD 11)
    # Define DEBUG in Debug mode
    if(CMAKE_BUILD_TYPE STREQUAL "Debug")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DDEBUG")
    endif()
    # enable openmp
    find_package(OpenMP)
    if (OPENMP_FOUND)
        set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
        set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    endif()

    find_package(OpenCV 3.0.0 REQUIRED)
    include_directories(OpenCV_INCLUDE_DIRS)
    find_package(VISP REQUIRED)
    include_directories(VISP_INCLUDE_DIRS)
endif()

set(src_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/src)
file(GLOB_RECURSE robocamcal_HEADERS ${src_ROOT}/*.hpp ${src_ROOT}/*.h)
#file(GLOB_RECURSE robocamcal_SOURCES ${src_ROOT}/*.cpp ${src_ROOT}/*.cc)

add_library(robocamcal INTERFACE)
target_sources(robocamcal INTERFACE ${robocamcal_HEADERS})

include_directories(${src_ROOT})

add_executable(hand_eye hand_eye.cpp)
target_link_libraries(hand_eye gflags robocamcal ${VISP_LIBRARIES} ${OpenCV_LIBRARIES})

add_executable(temp_test temp_test.cpp)
target_link_libraries(temp_test robocamcal ${VISP_LIBRARIES})