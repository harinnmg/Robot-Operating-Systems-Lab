# Turtlesim

The Turtlesim is a simple robot simulator used for familiarizing the concept of ros. A detailed description can be found at http://wiki.ros.org/turtlesim

The turtlesim can be started by running the roscore

```
roscore
```
In a new terminal, type
```
rosrun turtlesim turtlesim_node
```
It will open up a simulator. The turtlesim is a node. A node in ROS is a collection of topics, sevices, parameter servers etc.

To know about it, type

```
rosnode list
```
To know more
```
rosnode info /turtlesim
```

To know which all topics are published, we check
  ```
  rostopic list
  ```
  We can check the published topic, using following command (example)
  ```
  rostopic echo /turtle1/pose
  ```
  
  We can publish to a publisher using following command (edit)
  ```
  rostopic pub /turtle1/cmd_vel geometry_msgs/Twist "linear:
  x: 0.0
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: 0.0" 

```
We can also done using a python or c++ code, python code is attached for reference

Now we can use a launch file from a package to open turtlesim and motion code simultaneously

```
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node name="one" pkg="turtlesim" type="turtlesim_node" />
  <node name="topic_publisher" pkg="<package_name>" type="move.py" />
 </launch>
 
 ```
 





