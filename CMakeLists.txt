cmake_minimum_required(VERSION 3.1)

project(DSO)
set(${PROJECT_NAME}_VERSION 1.0.0)

#--------------------------
# Build configuration
#--------------------------
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif() 

set(CMAKE_CXX_STANDARD 11) 
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_definitions(-DCOMPILEDWITHC11)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -march=native -mtune=native")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=native -mtune=native")
add_compile_options(-Wall -Wextra -pedantic -Wno-unused-parameter -Wno-unused-variable)

#--------------------------
# Where to find modules
#--------------------------
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

#--------------------------
# required libraries
#--------------------------
find_package(SuiteParse REQUIRED)
find_package(Boost COMPONENTS system thread REQUIRED) 
find_package(Eigen3 REQUIRED)
find_package(Sophus REQUIRED)

#--------------------------
# Sources files
#--------------------------
include_directories(src) 

set(dso_SOURCE_FILES
src/FullSystem/FullSystem.cpp
src/FullSystem/FullSystemOptimize.cpp
src/FullSystem/FullSystemOptPoint.cpp
src/FullSystem/FullSystemDebugStuff.cpp
src/FullSystem/FullSystemMarginalize.cpp
src/FullSystem/Residuals.cpp
src/FullSystem/CoarseTracker.cpp
src/FullSystem/CoarseInitializer.cpp
src/FullSystem/ImmaturePoint.cpp
src/FullSystem/HessianBlocks.cpp
src/FullSystem/PixelSelector2.cpp
src/OptimizationBackend/EnergyFunctional.cpp
src/OptimizationBackend/AccumulatedTopHessian.cpp
src/OptimizationBackend/AccumulatedSCHessian.cpp
src/OptimizationBackend/EnergyFunctionalStructs.cpp
src/util/settings.cpp
src/util/Undistort.cpp
src/util/globalCalib.cpp
)

#--------------------------
# compile main library.
#--------------------------
add_library(${PROJECT_NAME} SHARED ${dso_SOURCE_FILES})
target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
    $<INSTALL_INTERFACE:>
    PRIVATE src)
    
target_include_directories(${PROJECT_NAME} PUBLIC ${EIGEN3_INCLUDE_DIR})
target_include_directories(${PROJECT_NAME} PUBLIC ${Sophus_INCLUDE_DIRS})
target_include_directories(${PROJECT_NAME} PUBLIC ${CSPARSE_INCLUDE_DIR} ${CHOLMOD_INCLUDE_DIR})
