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
  We can check the published topic, using
  ```
  rostopic echo /turtle1/pose
  ```
  
  

