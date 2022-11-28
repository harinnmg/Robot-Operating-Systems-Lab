#Experiment 6: Familiarization of Gazebo
## Theory
Gazebo is an open robotics simulator, where one can design and simulate robots. A robot will be associated with different predefined structures, sensors, actuators etc. So as seen in example 5, after adding basic shapes, we can add the capabilities in the form of plugins. 
plugin is a chunk of code that is compiled as a shared library and inserted into the simulation. The plugin has direct access to all the functionality of Gazebo through the standard C++ classes.

Plugins are useful because they:

 •	let developers control almost any aspect of Gazebo
 
 •	are self-contained routines that are easily shared
 
 •	can be inserted and removed from a running system
 
Previous versions of Gazebo utilized controllers. These behaved in much the same way as plugins, but were statically compiled into Gazebo. Plugins are more flexible, and allow users to pick and choose what functionality to include in their simulations

You should use a plugin when:

•	you want to programmatically alter a simulation

Ex: move models, respond to events, insert new models given a set of preconditions

•	you want a fast interface to gazebo, without the overhead of the transport layer

Ex: No serialization and deserialization of messages.

•	you have some code that could benefit others and want to share it

Plugin Types

There are currently 6 types of plugins

•	World

•	Model

•	Sensor

•	System

•	Visual

•	GUI

Each plugin type is managed by a different component of Gazebo. For example, a Model plugin is attached to and controls a specific model in Gazebo. Similarly, a World plugin is attached to a world, and a Sensor plugin to a specific sensor. The System plugin is specified on the command line, and loads first during a Gazebo startup. This plugin gives the user control over the startup process.

A plugin type should be chosen based on the desired functionality. Use a World plugin to control world properties, such as the physics engine, ambient lighting, etc. Use a Model plugin to control joints, and state of a model. Use a Sensor plugin to acquire sensor information and control sensor properties

## Procedure
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
7. Adding plugins: follow the link instructions.
https://classic.gazebosim.org/tutorials?tut=plugins_hello_world&cat=write_plugin
# Experiment 7: Create a custom world

1. Close all the terminals and open Gazebo
```gazebo```
2. Go to eidt>>bulding editor and create shapes your own. Save the shape and exit building editor
3. Save the world from file>>save to the location 'world' folder inside your package
4. Go to the launch folder inside the package and edit the code, replace <package_nmae> and <world_nmae> with yours

```
<?xml version="1.0"?>
<launch>
<param name="robot_description" command="cat $(find
<package_name>)/urdf/<urdf_name>" />
<arg name = "x" default = "0"/>
<arg name = "y" default = "0"/>
<arg name = "z" default = "0"/>
 <!-- World File -->
  <arg name="world_file" default="$(find package name)/world/world name"/>
<node name="spawn_urdf" pkg="gazebo_ros" type="spawn_model" output="screen"
args="-urdf -param robot_description -model my_first -x $(arg x) -y $(arg y) -z $(arg z)"/>

  <include file="$(find gazebo_ros)/launch/empty_world.launch">
    <arg name="use_sim_time" value="true"/>
    <arg name="debug" value="false"/>
    <arg name="gui" value="true" />
    <arg name="world_name" value="$(arg world_file)"/>
  </include>

</launch>
```
5. Do the procedure for roslaunch as we done previously 
