# Note
The package name is test2, and the package is having two extra folders, urdf and launch

# URDF Code
```
<?xml version="1.0"?>
<robot name="myfirst">
  <link name="base_link">
    <visual>
      <geometry>
        <cylinder length="0.6" radius="0.2"/>
      </geometry>
      <material name="blue">
        <color rgba="0 0 0.8 1"/>
      </material>
    </visual>
    <collision>
      <geometry>
        <cylinder length="0.6" radius="0.2"/>
      </geometry>
    </collision>
    <inertial>
      <mass value="1"/>
      <inertia ixx="0.145833" ixy="0" ixz="0" iyy="0.145833" iyz="0" izz="0.125"/>
    </inertial>
  </link>

  <gazebo reference="base_link">
    <material>Gazebo/Blue</material>
  </gazebo>
</robot>
```

# Launch file

```
import os
from ament_index_python.packages import get_package_share_directory
from launch import LaunchDescription
from launch.actions import IncludeLaunchDescription
from launch.launch_description_sources import PythonLaunchDescriptionSource
from launch_ros.actions import Node
from launch.actions import DeclareLaunchArgument, ExecuteProcess
import xml.etree.ElementTree as ElementTree


def generate_launch_description():
    # Get the package share directory
    pkg_share = get_package_share_directory('test2')

    # Define the path to the world file
    world_file_name = 'empty.world'
    world_path = os.path.join(pkg_share, 'urdf', world_file_name)

    # Define the path to the URDF file
    urdf_file_name = 'test.urdf'
    urdf_path = os.path.join(pkg_share, 'urdf', urdf_file_name)
    with open(urdf_path, 'rb') as f:
     xml_data = f.read()
    xml_parsed = ElementTree.fromstring(xml_data)
    return LaunchDescription([
        # Start Gazebo server with the world file
        DeclareLaunchArgument(name='use_sim_time', default_value='true', description='Use simulation (Gazebo) clock if true'),
        IncludeLaunchDescription(
            PythonLaunchDescriptionSource([os.path.join(
                get_package_share_directory('gazebo_ros'), 'launch', 'gzserver.launch.py')]),
            launch_arguments={'world': world_path}.items(),
        ),
        
        # Start Gazebo client
        IncludeLaunchDescription(
            PythonLaunchDescriptionSource([os.path.join(
                get_package_share_directory('gazebo_ros'), 'launch', 'gzclient.launch.py')]),
        ),

        Node(
             package='joint_state_publisher',
             executable='joint_state_publisher',
             name='joint_state_publisher',
             ),
 
        # Publish the URDF to the robot_description topic
        Node(
            package='robot_state_publisher',
            executable='robot_state_publisher',
            output='screen',
            arguments=[urdf_path]
        ),
        Node(
        package='rviz2',
        executable='rviz2',
        name='rviz2',
        output='screen',
        arguments=['-d', os.path.join(pkg_share, 'rviz', 'hello1.rviz')],
        ),
        # Spawn the robot entity
        Node(
            package="gazebo_ros",
            executable="spawn_entity.py",
            arguments=['-entity', 'simple_robot', '-topic', 'robot_description', "-x", "0.0", "-y", "0.0", "-z", "0.0"],
            output="screen"
        )
    ])

if __name__ == '__main__':
    generate_launch_description()
```
CMakeLists.txt
```
cmake_minimum_required(VERSION 3.8)
project(test2)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)
install(
  DIRECTORY launch urdf
  DESTINATION share/${PROJECT_NAME}
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

ament_package()
```

package.xml

```
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format3.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="3">
  <name>test2</name>
  <version>0.0.0</version>
  <description>TODO: Package description</description>
  <maintainer email="harinarayanan@todo.todo">harinarayanan</maintainer>
  <license>TODO: License declaration</license>

  <buildtool_depend>ament_cmake</buildtool_depend>

  <test_depend>ament_lint_auto</test_depend>
  <test_depend>ament_lint_common</test_depend>
<exec_depend>joint_state_publisher</exec_depend>
<exec_depend>robot_state_publisher</exec_depend>
<exec_depend>gazebo_ros</exec_depend>
<exec_depend>xacro</exec_depend>
  <export>
    <build_type>ament_cmake</build_type>
  </export>
</package>
```
