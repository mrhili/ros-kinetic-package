cd ~/catkin_ws/src

catkin_create_pkg hello_world std_msgs rospy

cd hello_world

mkdir scripts

cd scripts/

touch hello_world_publisher.py

touch hello_world_subscriber.py

*Inside the publisher write those lines

#!/usr/bin/env python
import rospy
from std_msgs.msg import String

def talker():
    pub = rospy.Publisher('hello_pub', String, queue_size=10)
    
    rospy.init_node('hello_world_publisher', anonymous=True)

    r = rospy.Rate(10) # 10hz

    while not rospy.is_shutdown():
        str = "hello world %s"%rospy.get_time()
        rospy.loginfo(str)
        pub.publish(str)
        r.sleep()

if __name__ == '__main__':
    try:
        talker()
    except rospy.ROSInterruptException: pass
-------------------------

*Inside the subscriber write

#!/usr/bin/env python
import rospy
from std_msgs.msg import String

def callback(data):
    rospy.loginfo(rospy.get_caller_id()+"I heard %s",data.data)
    
def listener():
    rospy.init_node('hello_world_subscriber', anonymous=True)
    rospy.Subscriber("hello_pub", String, callback)
    rospy.spin()
    
    
if __name__ == '__main__':
    listener()
    
    
-------------------

chmod +x hello_world_publisher.py
chmod +x hello_world_subscriber.py


cd ~/catkin_ws
catkin_make


echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc

*In terminal 1
roscore
*In terminal 2
rosrun hello_world hello_world_publisher.py
*3 In terminal 3
rosrun hello_world hello_world_subscriber.py

