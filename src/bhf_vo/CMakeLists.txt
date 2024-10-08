cmake_minimum_required(VERSION 3.8)
project(bhf_vo)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(image_transport REQUIRED)
find_package(OpenCV REQUIRED)
find_package(rosidl_default_generators REQUIRED)

# Declare the message files to be built
rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/VisualCustom.msg"
  DEPENDENCIES geometry_msgs  # Add other dependencies if needed
)
if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

add_executable(bhf_vo_node src/bhf_vo_node.cpp)
ament_target_dependencies(bhf_vo_node rclcpp std_msgs sensor_msgs cv_bridge image_transport OpenCV)

# Link the generated message files
target_link_libraries(bhf_vo_node
  ${PROJECT_NAME}__rosidl_typesupport_cpp)

install(TARGETS bhf_vo_node
DESTINATION lib/${PROJECT_NAME})

ament_package()
