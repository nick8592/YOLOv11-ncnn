cmake_minimum_required(VERSION 3.10)
project(YOLOv11-ncnn)

set(ncnn_DIR "${CMAKE_SOURCE_DIR}/ncnn/build/install/lib/cmake/ncnn")
find_package(OpenCV REQUIRED)
find_package(ncnn REQUIRED)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

add_executable(yolo11 ${CMAKE_SOURCE_DIR}/src/yolo11.cpp)
target_link_libraries(yolo11 ncnn ${OpenCV_LIBS})

add_executable(yolo11_seg ${CMAKE_SOURCE_DIR}/src/yolo11_seg.cpp)
target_link_libraries(yolo11_seg ncnn ${OpenCV_LIBS})
