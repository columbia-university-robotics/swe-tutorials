# ROS Tutorial #4

This tutorial is written for the Columbia University Robotics Club (CURC). The goal is to provide a general overview of ROS as well as how to write some simple publishers and subscribers. 

# Publish-subscribe pattern

Our robots must send data (or messages) between its components. For example, a Jetson Nano running Ubuntu might want to send an Arduino Uno the speed of the motor that the Jetson calculated using a live camera feed. In this case, the Jetson would be the sensor of the message, and the Arduino would be the subscriber. 

Another example would be if we have a piece of software written for reading the camera feed, and we have another piece of code that runs a simple object detection algorithm on the images captured, the image pipeline would be the publisher of the image message, and the machine learning algorithm would be the subscriber. 

ROS very much rely on this publish-subscribe design pattern. However, it's not the only way that a robot can communicate. It's one of a larger message-oriented middleware system. A few advantages of this approach are scalability and modularity. 

# Write a simple "Hello World" Publisher

Now, let's try to write some Python code that simply publishes the string "hello world" and the current timestamp.

I would suggest you to follow along with with tutorial before cloning this repo and running the code.  

## Create a ROS Package. 

Before we get started, let's look at what are packages in ROS. The following is taken from ROS's official documentation: 

"Software in ROS is organized in packages ... The goal of these packages it to provide this useful functionality in an easy-to-consume manner so that software can be easily reused. In general, ROS packages follow a "Goldilocks" principle: enough functionality to be useful, but not too much that the package is heavyweight and difficult to use from other software."

First, create a directory for your project, I called mine ROS tutorial. 
    
    mkdir ROS-tutorial

Then, create a `src` directory. This is where your source code will live. 
    
    cd ROS-tutorial
    mkdir src

Now we will use a built-in ROS command line tool to create our new package. 
    
    catkin_create_pkg simple_number_pub std_msgs rospy

    // this message is the equavelent of this: 
    catkin_create_pkg <package_name> [depend1] [depend2]

Once we have a project, let's build it! 

	cd ..
	catkin_make

You will see a bunch of texts on your command line window. Hopefully no errors.

What you have done up to this point is setting up a ROS project and package on your computer. Now you perhaps begin to realize that calling ROS an 'operating system' perhaps is a misnomer. In fact, it looks and works not quite like an operating system. It's generally considered an open-source robotics middleware. 

## The Python Code

Once we have the package setup, let's write some code in Python. This code that we write is a python script, in ROS, it's generally called a node. A package can have multiple nodes. Imagine a ROS package that manages the camera feed. You might need a node that captures the raw image data and publishes those as RGB images. You might need another node that can be executed when you need to apply a grayscale filter. 

In short, nodes help us to keep the code organized. 

First, let's navigate to the directory of the package. 

	cd src/simple_number_pub 

Make a new directory called `scripts` This is where all of our Python scripts for this package will live. 

	mkdir scripts
	cd scripts

Let's make a new file with your favorite text editor, and call it `number_pub.py`

Please refer to this Github link for the code: [code]()

	#!/usr/bin/env python

	import rospy
	from std_msgs.msg import String

	def talker():

    	pub = rospy.Publisher('chatter', String, queue_size=10)
    
    	rospy.init_node('talker', anonymous=True)
    
    	rate = rospy.Rate(10) # 10hz
    
    	while not rospy.is_shutdown():

        	hello_str = "hello world %s" % rospy.get_time()
        	rospy.loginfo(hello_str)
        	pub.publish(hello_str)
        	rate.sleep()

	if __name__ == '__main__':
    	try:
        	talker()
    	except rospy.ROSInterruptException:
        	pass

### Breaking down the code a little bit

- Unlike C++, running python code as an executable is not very difficult. This is why this line: `#!/usr/bin/env python` is very important. It allows this script to be run as an executable. 

- The `if __name__ == '__main__` is analogous to Java's or C's main method. 

- We must give our code a name and our topic a name as well. 

(I will go over this code in detail during CURC's meetings)

### Moving on...

Then, we need to turn this script into something that ROS can execute. We simply need one command for that: 

	chmod u+x number_pub.py

(Make sure that you are still in the scripts directory of your simple_number_pub package)

## Running the node. 

Return to the directory of this project. 

	catkin_make
	source devel/setup.bash

This is where the magic happens:

	rosrun simple_number_pub number_pub.py

You will see the screen logging some information. If you see hello world and the current timestamp. you have successfully completed this tutorial. 

# Suggested Reading: 

Please take a look at these articles for more detailed information on the content described in this tutorial: 

[Packages](http://wiki.ros.org/Packages)
[Understanding Topics](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics)
[Understanding Nodes](http://wiki.ros.org/ROS/Tutorials/UnderstandingNodes)
[Simple Publisher](http://wiki.ros.org/rospy_tutorials/Tutorials/WritingPublisherSubscriber)

**Please contact neil at neil.nie@columbia.edu for any questions, comments, suggestions, or concerns. Thank you!** 