cmake_minimum_required(VERSION 3.8)
project(andino_base)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# ROS 2 packages
find_package(ament_cmake REQUIRED)
find_package(hardware_interface REQUIRED)
find_package(rclcpp REQUIRED)
find_package(pluginlib REQUIRED)
find_package(Threads REQUIRED)

# LibSerial via pkg-config
find_package(PkgConfig REQUIRED)
pkg_check_modules(SERIAL libserial REQUIRED)

include_directories(
  ${SERIAL_INCLUDE_DIRS}
)

link_directories(
  ${SERIAL_LIBRARY_DIRS}
)

# Add subdirectories (these already define the targets)
add_subdirectory(src)
add_subdirectory(applications)

# Optional clang-format
if(BUILD_TESTING)
  find_package(ament_cmake_clang_format REQUIRED)
  ament_clang_format(CONFIG_FILE ${CMAKE_CURRENT_SOURCE_DIR}/.clang-format)
endif()

# Export plugin description for pluginlib
pluginlib_export_plugin_description_file(hardware_interface andino_hw.xml)

ament_package()
