#1.cmake verson，指定cmake的最小版本号 
cmake_minimum_required(VERSION 2.8)

find_package(OpenCV QUIET COMPONENTS core highgui imgproc imgcodecs)
if(NOT OpenCV_FOUND)
    find_package(OpenCV REQUIRED COMPONENTS core highgui imgproc)
endif()

#3.set environment variable，设置环境变量
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -fopenmp")

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/include/ncnn)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/3rdparty/include/ncnn)

#add_executable(mtcnn ../src/*)
#target_link_libraries(mtcnn ncnn ${OpenCV_LIBS})

#5.library目录及name名称
#ink_directories(${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/lib)
list(APPEND MTCNN_LINKER_LIBS ncnn )

#6.source directory源文件目录
file(GLOB MTCNN_SRC ${CMAKE_CURRENT_LIST_DIR}/src/*.h
                    ${CMAKE_CURRENT_LIST_DIR}/src/*.cpp)
set(MTCNN_COMPILE_CODE ${MTCNN_SRC})

#7.1.add executable file，编译为可执行文件
add_executable(mtcnn_ncnn ${MTCNN_COMPILE_CODE})
#7.2.add library file，编译为动态库
#add_library(mtcnn_ncnn SHARED ${MTCNN_COMPILE_CODE})

#8.add link library，添加工程所依赖的库
target_link_libraries(mtcnn_ncnn ${OpenCV_LIBS} ${MTCNN_LINKER_LIBS})
