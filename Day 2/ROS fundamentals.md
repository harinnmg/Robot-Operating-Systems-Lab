# Creating ROS Workspace

 ```
 >> mkdir cd_catkin_ws/src
 >> cd catkin_ws
 >> catkin_make
 ```
 
# Creating ROS Package
```
>> cd catkin_ws/src
>> catkin_create_pkg <package_name> rospy roscpp
```
# Publisher code

```
#!/usr/bin/env python
import rospy
from std_msgs.msg import string
def publishMethod():
 pub=rospy.publish('talker',string,queue_size=10)
 rospy.init_node('publish_node',anonymous=true)
 rate=rospy.rate(10)
while not rospy.is_shutdown():
 
 publishstring= "hi"
 rospyloginfo('data is being sent')
 pub.publish(publishstring)
 rate.sleep()

if_name_=='_main_':
try:
  publishMethod()
 except rospy.ROSInterruptException:
 pass
 ```
 
