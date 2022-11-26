1. Create a new package inside your workspace src, and create three folders inside; urdf,launch and world.
2. Inside urdf folder, create a text file with .urdf extension and copy the following code.

Urdf file

```
<?xml version="1.0"?>
<robot name="myfirst">
<link name="base_link">
<inertial>
<mass value="5"/>
<origin rpy="0 0 0" xyz="0 0 0"/>
<inertia ixx="0" ixy="0" ixz="0" iyy="0.1" iyz="0" izz="0"/>
</inertial>
<collision>
<geometry>
<cylinder length="0.5" radius="0.5"/>
</geometry>
</collision>
<visual>
<geometry>
<cylinder length="0.5" radius="0.5"/>
</geometry>
</visual>
</link>

</robot>
```
3. Inside the launch folder, create a text file with extension .launch, copy the following code, replace <package_name> with your package name and <urdf_name> with the urdf name.
Launch file

```
<?xml version="1.0"?>
<launch>
<param name="robot_description" command="cat $(find
<package_name>)/urdf/<urdf_name>.urdf" />
<arg name = "x" default = "0"/>
<arg name = "y" default = "0"/>
<arg name = "z" default = "0"/>
<node name="spawn_urdf" pkg="gazebo_ros" type="spawn_model" output="screen"
args="-urdf -param robot_description -model my_first -x $(arg x) -y $(arg y) -z $(arg z)"/>

  <include file="$(find gazebo_ros)/launch/empty_world.launch">
    <arg name="use_sim_time" value="true"/>
    <arg name="debug" value="false"/>
    <arg name="gui" value="true" />
    <arg name="world_name" value="true"/>
  </include>

</launch>
```
4. Go to workspace and catkin_make
5. Apply the command ```source devel/setup.bash```
6. Run the command ```roslaunch <package_name> <launch_code with extension>``` replace  <package_name> with your package name and <launch_code with extension> with your launch file name with .launch extension. You will see the cylinder is spawned in Gazebo.
#Create a custom world

```
<?xml version="1.0"?>
<launch>
<param name="robot_description" command="cat $(find
urdftest1)/urdf/name.urdf" />
<arg name = "x" default = "0"/>
<arg name = "y" default = "0"/>
<arg name = "z" default = "0"/>
 <!-- World File -->
  <arg name="world_file" default="$(find urdftest1)/world/house.world"/>
<node name="spawn_urdf" pkg="gazebo_ros" type="spawn_model" output="screen"
args="-urdf -param robot_description -model my_first -x $(arg x) -y $(arg y) -z $(arg z)"/>

  <include file="$(find gazebo_ros)/launch/empty_world.launch">
    <arg name="use_sim_time" value="true"/>
    <arg name="debug" value="false"/>
    <arg name="gui" value="true" />
    <arg name="world_name" value="true"/>
  </include>

</launch>
```
