cmake_minimum_required(VERSION 2.8)
project(console_bridge)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

find_package(Boost REQUIRED COMPONENTS system thread)

find_package(catkin REQUIRED)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  DEPENDS Boost
)

if(MSVC OR MSVC90 OR MSVC10)
  set(MSVC ON)
endif()

if(MSVC)
  add_definitions(-DBOOST_ALL_NO_LIB)
endif()

include_directories(SYSTEM ${Boost_INCLUDE_DIR})
include_directories(include)
link_directories(${Boost_LIBRARY_DIRS})

add_library(${PROJECT_NAME} src/console.cpp)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES})

install(TARGETS ${PROJECT_NAME}
  DESTINATION lib/)

install(DIRECTORY include/
  DESTINATION include
  FILES_MATCHING PATTERN "*.h")
