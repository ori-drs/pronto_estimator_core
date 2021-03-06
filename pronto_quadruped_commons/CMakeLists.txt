cmake_minimum_required(VERSION 3.0.2)
project(pronto_quadruped_commons)

add_compile_options(-Wall)

find_package(Eigen3 REQUIRED)

find_package(catkin REQUIRED)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(BUILD_DOC false  CACHE BOOL "Build Doxygen documentation")

if(${BUILD_DOC})
    # check if Doxygen is installed
    find_package(Doxygen QUIET)
    if (DOXYGEN_FOUND)
        # set input and output files
        set(DOXYGEN_IN ${CMAKE_CURRENT_SOURCE_DIR}/doc/Doxyfile.in)
        set(DOXYGEN_OUT ${CMAKE_CURRENT_SOURCE_DIR}/doc/Doxyfile)

        # request to configure the file
        configure_file(${DOXYGEN_IN} ${DOXYGEN_OUT} @ONLY)

        # note the option ALL which allows to build the docs together with the application
        add_custom_target( doc_doxygen ALL
            COMMAND ${DOXYGEN_EXECUTABLE} ${DOXYGEN_OUT}
            WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
            COMMENT "Generating API documentation with Doxygen"
            VERBATIM )
    else (DOXYGEN_FOUND)
        message(WARNING "Doxygen need to be installed to generate the Doxygen documentation")
    endif (DOXYGEN_FOUND)
endif()

catkin_package(
   INCLUDE_DIRS include ${EIGEN3_INCLUDE_DIRS}
   CATKIN_DEPENDS
   LIBRARIES 
   DEPENDS
)

# Include directories
include_directories(include ${EIGEN3_INCLUDE_DIRS})

# Install iit-rbd globally
install(DIRECTORY include/iit
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION})

# Install pronto_quadruped_commons locally
install(DIRECTORY include/${PROJECT_NAME}/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})
