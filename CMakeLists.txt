##################################################################################
# CRT Compiler CMakeLists.txt
##################################################################################

cmake_minimum_required(VERSION 3.12)
enable_testing()

#-------------------------------------------------------------------------
# User Edits
#-------------------------------------------------------------------------

set(SRC_FILES
  # Include your source files here!
  # Don't include the directory, only the file name
)

set(TEST_FILES
  # Include your test files here!
  # Don't include the directory, only the file name
)

#=========================================================================
# !!!!DO NOT TOUCH THE FOLLOWING CODE!!!!
#=========================================================================


#-------------------------------------------------------------------------
# Functions
#-------------------------------------------------------------------------

##########################
# include_directory_files
##########################
# Includes all files in directory (recursively) to ensure we have
# all relevant header files

function(include_directory_files FILE_PATH)
  include_directories(${FILE_PATH})
  FILE(GLOB children RELATIVE ${FILE_PATH} ${FILE_PATH}/*)
  FOREACH(child ${children})
    IF(IS_DIRECTORY ${FILE_PATH}/${child})
      include_directory_files(${FILE_PATH}/${child})
    ENDIF()
  ENDFOREACH()
endfunction()

##########################
# get_leaf
##########################
# Gets the leaf file of a path.

function(get_leaf VARNAME FILE_NAME)
  string(REGEX REPLACE "[^/]*/" "" NAME_WITHOUT_EXT ${FILE_NAME})
  set(${VARNAME} "${NAME_WITHOUT_EXT}" PARENT_SCOPE)
endfunction()

##########################
# remove_extension
##########################
# Removes the file extension

function(remove_extension VARNAME FILE_NAME)
  string(REGEX REPLACE "\\.[^.]*$" "" NAME_WITHOUT_EXT ${FILE_NAME})
  set(${VARNAME} "${NAME_WITHOUT_EXT}" PARENT_SCOPE)
endfunction()

#-------------------------------------------------------------------------
# Set up environment
#-------------------------------------------------------------------------

# Turn off compiler check

set(CMAKE_C_COMPILER_WORKS TRUE)
set(CMAKE_CXX_COMPILER_WORKS TRUE)

# Global compiler flags

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -O3 -g")

#---------------------------------------------------------------------------
# Compiler setup
#---------------------------------------------------------------------------

# use g++
message("Setting the compiler to g++")
set(CMAKE_CXX_COMPILER g++)

# Declare project
project(build_framework LANGUAGES CXX)

# Set up directories
set(SRC_DIR "${CMAKE_CURRENT_SOURCE_DIR}/src")
set(TEST_DIR "${CMAKE_CURRENT_SOURCE_DIR}/tests")
set(INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/include")

# Include path to source and header files
include_directory_files(${INCLUDE_DIR})
include_directory_files(${SRC_DIR})

# Link source files into a library
unset(LIB_SRCS)
foreach(SRC_FILE ${SRC_FILES})
  set(LIB_SRCS ${LIB_SRCS} "${SRC_DIR}/${SRC_FILE}")
endforeach(SRC_FILE)

add_library(${PROJECT_NAME} ${LIB_SRCS})

#---------------------------------------------------------------------------
# Compile targets
#---------------------------------------------------------------------------

add_custom_target(all_tests COMMAND ctest "--timeout" "10" ${CTEST_OUTPUT_ON_FAILURE})

foreach(TEST_FILE ${TEST_FILES})

    get_leaf(TEST_TEMP ${TEST_FILE})
    remove_extension(TEST_BIN ${TEST_TEMP})
    set(TEST_SRC "${TEST_DIR}/${TEST_FILE}")

    # Add executable
    add_executable(${TEST_BIN} EXCLUDE_FROM_ALL ${TEST_SRC})
    target_link_libraries(${TEST_BIN} m ${PROJECT_NAME})
    add_dependencies(all_tests ${TEST_BIN})
    
    # Add test
    add_test(${TEST_BIN} ${TEST_BIN})

endforeach(TEST_FILE)
