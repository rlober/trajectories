# This file is part of reach-client.
# Copyright (C) [your institution here]
# author(s): [your name here]
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

# Make sure we are working with at least CMake 2.8.12
cmake_minimum_required(VERSION 2.8.12)

# Initiate the project
PROJECT(GTTraj CXX)

# Make sure you have a C++11 compatible compiler
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
else()
    message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
 endif()

 # Build as Release (Change Release to Debug for better debugging symbols)
set(CMAKE_BUILD_TYPE Release)

set(VARS_PREFIX "GTTraj")
# Set the project version.
set(${PROJECT_NAME}_MAJOR_VERSION 1)
set(${PROJECT_NAME}_MINOR_VERSION 0)
set(${PROJECT_NAME}_PATCH_VERSION 0)
set(${PROJECT_NAME}_VERSION ${${PROJECT_NAME}_MAJOR_VERSION}.${${PROJECT_NAME}_MINOR_VERSION}.${${PROJECT_NAME}_PATCH_VERSION})

# Add some helpful CMake functions
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules)

# Find OcraIcub
find_package(Eigen REQUIRED)

# FILE(GLOB folder_header include/gttraj/*.h)
# Tell the compiler where to look for all other headers
include_directories(
${PROJECT_SOURCE_DIR}/include
${Eigen_INCLUDE_DIRS}
)

add_library(gttraj SHARED src/Path.cpp src/Trajectory.cpp)

install(DIRECTORY ${PROJECT_SOURCE_DIR}/include
        DESTINATION ${CMAKE_INSTALL_PREFIX}
        FILES_MATCHING PATTERN *.h*)

install(TARGETS gttraj
        EXPORT GTTraj
        DESTINATION lib)

set_property(GLOBAL APPEND PROPERTY ${VARS_PREFIX}_TARGETS gttraj)
set_property(GLOBAL APPEND PROPERTY PACKAGE_${VARS_PREFIX}_INCLUDEDIR ${CMAKE_INSTALL_PREFIX}/include)





include_directories(gttraj_INCLUDE_DIRS)
# Add the example executable (binary)
add_executable(gttraj-tests src/Test.cpp)

# Link to the appropriate libs
target_link_libraries(gttraj-tests
gttraj
)

include_directories(gttraj_INCLUDE_DIRS)
# Add the example executable (binary)
add_executable(gttraj-example src/Example.cpp)

# Link to the appropriate libs
target_link_libraries(gttraj-example
gttraj
)

# Install to the lib/ and bin/ directories if installed.
install(TARGETS gttraj-tests DESTINATION bin)
install(TARGETS gttraj-example DESTINATION bin)

# Add an uninstallation target so you can just run - make uninstall - to remove the binary.
include(InstallBasicPackageFiles)
get_property(PACKAGE_${VARS_PREFIX}_INCLUDEDIR GLOBAL PROPERTY PACKAGE_${VARS_PREFIX}_INCLUDEDIR)
install_basic_package_files(GTTraj      VARS_PREFIX ${VARS_PREFIX}
                                        VERSION ${${VARS_PREFIX}_VERSION}
                                        COMPATIBILITY SameMajorVersion
                                        TARGETS_PROPERTY ${VARS_PREFIX}_TARGETS
                                        NO_CHECK_REQUIRED_COMPONENTS_MACRO
                                    )
include(AddUninstallTarget)
