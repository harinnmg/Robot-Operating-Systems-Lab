```
import os
from ament_index_python.packages import get_package_share_directory
from launch import LaunchDescription
from launch_ros.actions import Node
import xml.etree.ElementTree as ElementTree

def generate_launch_description():
    # Get the package share directory
    pkg_share = get_package_share_directory('test1')

    # Define the path to the URDF file
    urdf_file_name = 'first.urdf'
    urdf_path = os.path.join(pkg_share, 'urdf', urdf_file_name)

    with open(urdf_path, 'rb') as f:
        xml_data = f.read()
    xml_parsed = ElementTree.fromstring(xml_data)

    return LaunchDescription([
        Node(
            package='joint_state_publisher',
            executable='joint_state_publisher',
            name='joint_state_publisher',
        ),

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
    ])

if __name__ == '__main__':
    generate_launch_description()
```
