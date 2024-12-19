cmake_minimum_required(VERSION 2.8.3)
project(aliengo_sdk)

# Set the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Include and link directories
include_directories(include)
link_directories(lib)

# Add compile options
add_compile_options(-std=c++11)

# Set extra libraries
set(EXTRA_LIBS -pthread aliengo_comm lcm) 
# Note: If the library is named 'libaliengo_comm.so', just specifying 'aliengo_comm' (without prefix and suffix) is enough
# as long as it is found in the linked directory.

# Add executables
add_executable(position_example examples/position_example.cpp)
target_link_libraries(position_example ${EXTRA_LIBS})

add_executable(velocity_example examples/velocity_example.cpp)
target_link_libraries(velocity_example ${EXTRA_LIBS})

add_executable(torque_example examples/torque_example.cpp)
target_link_libraries(torque_example ${EXTRA_LIBS})

add_executable(walk_example examples/walk_example.cpp)
target_link_libraries(walk_example ${EXTRA_LIBS})

add_executable(sdk_lcm_server_high examples/high_lcm_server.cpp)
target_link_libraries(sdk_lcm_server_high ${EXTRA_LIBS})

add_executable(sdk_lcm_server_low examples/low_lcm_server.cpp)
target_link_libraries(sdk_lcm_server_low ${EXTRA_LIBS})

add_executable(sdk_lcm_server examples/lcm_server.cpp)
target_link_libraries(sdk_lcm_server ${EXTRA_LIBS})

# ---------------------------------------------------------
# Installation Instructions
# ---------------------------------------------------------

# Install the executables into bin/
install(TARGETS
    position_example
    velocity_example
    torque_example
    walk_example
    sdk_lcm_server_high
    sdk_lcm_server_low
    sdk_lcm_server
    RUNTIME DESTINATION bin
)

# Install the headers into include/
install(DIRECTORY include/ DESTINATION include)

# Install the shared library if it's part of this project
# If libaliengo_comm.so is provided by you in the 'lib' directory, you can also install it.
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/lib/libaliengo_comm.so DESTINATION lib)

configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY
)

add_custom_target(uninstall
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake
)

# Optionally, install the lcm library if it's local (if not system-provided)
# If lcm is installed from system packages, skip this. Otherwise:
# install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/lib/liblcm.so DESTINATION lib)

# ---------------------------------------------------------
# Additional configurations for external use
# ---------------------------------------------------------
# If you want external CMake projects to find this package easily, 
# you can provide a config file. This is a more advanced topic, but
# for now installing binaries, headers, and libs is a good start.
