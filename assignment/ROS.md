
# ROS

------

## 1 Description
### Robot operation system安装步骤


------

## 2 Procedure

#### 2.1 Setup sources.list
	sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
	
#### 2.2 Set up keys
	sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116
#### 2.3 Installation
	sudo apt-get update
	sudo apt-get install ros-jade-desktop-full
	#To find available packages, use: 
	apt-cache search ros-jade
#### 2.4 Initialize rosdep
	sudo rosdep init
	rosdep update
#### 2.5 Environment setup
	source /opt/ros/jade/setup.bash
#### 2.6 Getting rosinstall
	sudo apt-get install python-rosinstall
	
