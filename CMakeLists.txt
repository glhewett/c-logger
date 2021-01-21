cmake_minimum_required(VERSION 3.17)
set(LIB_NAME logger)
project(${LIB_NAME} C)

set(CMAKE_C_STANDARD 90)

option(build_tests "Build all of own tests" OFF)
option(build_examples "Build example programs" OFF)
option(build_docs "Build doxygen documentation" OFF)
#include_directories(src)

set( ${LIB_NAME}_srcs
        src/logger.c
        src/logger.h
        src/loggerconf.c
        src/loggerconf.h)

add_library(${LIB_NAME}  ${${LIB_NAME}_srcs} )

install(TARGETS ${LIB_NAME} EXPORT ${LIB_NAME}Targets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION lib
        INCLUDES destination include)

install(EXPORT ${LIB_NAME}Targets
        FILE ${${LIB_NAME}Targets}.cmake
        NAMESPACE ${LIB_NAME}
        DESTINATION lib/cmake/${LIB_NAME})

### Document
if(build_docs)
    add_custom_target(doc COMMAND "doxygen" "${PROJECT_SOURCE_DIR}/doc/Doxyfile")
endif()

### Test
if(build_tests)
    enable_testing()
    add_subdirectory(test)
endif()

### Example
if(build_examples)
    add_subdirectory(example)
endif()
