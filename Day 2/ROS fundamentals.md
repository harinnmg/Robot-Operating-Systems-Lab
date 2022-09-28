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
from std_msgs.msg import String
def publishMethod():
 pub=rospy.Publisher('talker',String,queue_size=10)
 rospy.init_node('publish_node',anonymous=True)
 rate=rospy.Rate(10)
 while not rospy.is_shutdown():
 
  publishstring= "hi"
  rospy.loginfo('data is being sent')
  pub.publish(publishstring)
  rate.sleep()

if __name__ == '__main__':
    try:
        publishMethod()
    except rospy.ROSInterruptException:
        pass

 ```
 
# Subscriber code

```
#!/usr/bin/env python
import rospy
from std_msgs.msg import String

def callback(data):
    rospy.loginfo(rospy.get_caller_id() + "I heard %s", data.data)
    
def listener():

   
    rospy.init_node('listener', anonymous=True)

    rospy.Subscriber("chatter", String, callback)

    # spin() simply keeps python from exiting until this node is stopped
    rospy.spin()

if __name__ == '__main__':
    listener()
    
```
