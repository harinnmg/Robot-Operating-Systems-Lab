# ROS Workspace

 ```
 
 >> cd catkin_ws
 >> catkin_make
 ```
 
# Creating ROS Package
```
>> cd catkin_ws/src
>> catkin_create_pkg <package_name> rospy roscpp
>>catkin_make
```
Now create a new text file inside the package and copy the following code to it. Name it as "talker.py"
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

Open a new text file inside the package and name "subcriber.py"

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
* After completion of this, do the following commands
```
chmod a+rw talker.py
chmod a+rw subsciber.py
```
# Running a ROS Master
Next is to run a ros master in a terminal in the directory catkin_ws
```
roscore
```
# Communication between nodes
Open a new terminal and type following commands
```
source devel/setup.bash
rosrun <package_name> talker.py
```
Open another terminal and type following commands
```
source devel/setup.bash
rosrun <package_name> subscriber.py
```
# Publishing through terminal
```
>>rostopic pub /topic_name std_msgs/String hello
```
# Publisher code in c++
```
#include "ros/ros.h"
#include "std_msgs/String.h"

#include <sstream>


int main(int argc, char **argv)
{
 
  ros::init(argc, argv, "talker");

 
  ros::NodeHandle n;

 
  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter", 1000);

  ros::Rate loop_rate(10);


  int count = 0;
  while (ros::ok())
  {
   
    std_msgs::String msg;

    std::stringstream ss;
    ss << "hello world " << count;
    msg.data = ss.str();

    ROS_INFO("%s", msg.data.c_str());


    chatter_pub.publish(msg);

    ros::spinOnce();

    loop_rate.sleep();
    ++count;
  }


  return 0;
}
```
# Subscriber Node in C++
In the SRC of 
```
#include "ros/ros.h"
#include "std_msgs/String.h"


void chatterCallback(const std_msgs::String::ConstPtr& msg)
{
  ROS_INFO("I heard: [%s]", msg->data.c_str());
}

int main(int argc, char **argv)
{
 
  ros::init(argc, argv, "listener");


  ros::NodeHandle n;


  ros::Subscriber sub = n.subscribe("chatter", 1000, chatterCallback);


  ros::spin();

  return 0;
}
```
Then add following lines in cMakeLists.txt inside src
```
 
 add_executable(talker src/talker.cpp)
add_dependencies(talker ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
 target_link_libraries(talker
 ${catkin_LIBRARIES}
 )
 
add_executable(listner src/listner.cpp)
add_dependencies(listner ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
 target_link_libraries(listner
 ${catkin_LIBRARIES}
 )

 ```
 
