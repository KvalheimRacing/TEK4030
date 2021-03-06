# Assignment 1 User Guide & Requirements

In this assignment we will work with simulation. We will simulate the PD controller with gravity compensation using **ROS**, an open-source, meta-operating system for robots.

<div align="center">
   <br>
  <img src="img\ROS-Development-BlogPost-01-ARTC+Update.png"><br><br>
</div>

ROS provides all the services you would expect from an operating system, including hardware abstraction, low-level device control, implementation of commonly-used functionality, message-passing between processes, and package management. It also provides tools and libraries for obtaining, building, writing, and running code across multiple computers.

It is advisable to skim trough [this](http://www.robotics.stanford.edu/~ang/papers/icraoss09-ROS.pdf) paper, and the first couple of [these tutorials](http://wiki.ros.org/ROS/Tutorials) to get a quick understanding of how ROS works.



## Environments


<div align="center">
   <br>
  <img src="img\vmware-horizon-cloud-overview-video.jpg"><br><br>
</div>

To save time on installation and setup config, we have allocated dedicated Ubuntu (16.04) machines with ROS installed. You can log in to these machines using [VMware Horizon client](https://my.vmware.com/web/vmware/info?slug=desktop_end_user_computing/vmware_horizon_clients/4_0) . Add the server: `vdi-apcon-prod.uio.no` and then login using your university credentials.

Additionally if you use a US keyboard layout you can use a [browser](https://vdi-apcon-prod.uio.no)-[based](http://ros.ifi.uio.no) web client (there are bugs with other keyboard layouts).

All Linux and Mac machines at IFI has a VMware Horizon client installed with the server already added, so you can work from there if needed.

You can also install ROS on your own machine by following [this](http://wiki.ros.org/melodic/Installation) guide.


## Collaboration and assignment work


<div align="center">
   <br>
  <img src="img\branching.png"><br><br>
</div>

Since your work on the dedicated linux machines may be lost, and to prepare you for collaboration in the final assignment, we will use **Git** for the work in this assignment.

Git is a distributed version control system for tracking changes in computer files, and coordinating work on those files among multiple people.

If you are new to Git you should skim though [this UiO introduction](http://www.uio.no/tjenester/it/maskin/filer/versjonskontroll/github.html)
and watch [this 20min video](https://www.youtube.com/watch?v=Y9XZQO1n_7c).

Udacity has also a very good free [GitHub & Collaboration](https://www.udacity.com/course/github-collaboration--ud456) course. Here you get to try out what you have watched and read about, with very good step by step guides. This is highly recommended.

As a test that you are up to speed, and to get your own GitHub encyclopedia, *fork* this [GitHub Cheat Sheet](https://github.com/KvalheimRacing/github-cheat-sheet) repo to your own account at [github.uio.no](github.uio.no).


<div align="center">
   <br>
  <img src="img\unfurl_why_slack.png"><br><br>
</div>

For communicating with each other, a sub-group or everyone in the course, we have created a [Slack team](unik4490.slack.com). See invite on UiO mail.
You can use the slack channel `assignment 1` for general questions about the assignment, and the channels `ros` and `control_theory` for discussion.

**Slack** works in a browser, but it's highly recommended to [download](https://slack.com/downloads) it as an app on your computer (there is also as an app for android/iOS). You can get a quick intro to slack by watching [these](https://youtu.be/9RJZMSsH7-g) [three](https://youtu.be/W8_tGC8pNvI) [videos](https://youtu.be/gvJAcElFMUU).



## Setup

<div align="center">
   <br>
  <img src="img\maxresdefault.jpg"><br><br>
</div>

The dedicated ubuntu (16.04) machines should all have ros *Kinetic*. Check by running `rosversion -d` in your terminal.

In order for the system to find the path to the ROS installation, you need to source it. This can be done by editing the .bashrc file in your user folder and adding the path to ROS:

```
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
*Substitute `Kinetic` with `Melodic` if you download yourself.*

When working with ROS is usual to use a [Catkin Workspace](http://wiki.ros.org/catkin/workspaces). A workspace is ROS's way of building several related packages and develop new software. We will create a new workspace to store the necessary packages for this course:

```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace
```
Even though the workspace is empty (there are no packages in the 'src' folder, just a single CMakeLists.txt) you can still "build" the workspace:

```
cd ~/catkin_ws/
catkin_make
```
*`catkin_make` is a ROS build tool and is used to build all packages in ROS. When editing C++ code you need to build your workspace like this every time for the changes to take effect.*

Further we need to source the catkin_ws so our ROS packages can be found by ROS and Ubuntu

```
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```


You can test if the system works by launching `roscore` in the terminal. If it runs successfully, the setup is complete.

We can now clone the necessary repositories in order to work with the robot. All packages needs to be placed in the src folder:

```
cd ~/catkin_ws/src
git clone https://github.uio.no/INF3480/crustcrawler_hardware.git
git clone https://github.uio.no/INF3480/crustcrawler_hardware.git
git clone --branch 3.5.4 https://github.com/ROBOTIS-GIT/DynamixelSDK.git
```

Before building the packages we need to download the necessary ROS dependencies. This can be done by using the built-in tool called [rosdep](http://wiki.ros.org/rosdep).

```
$ cd ~/catkin_ws
$ rosdep install --from-paths src --ignore-src -r -y

```
Once this is done we can build our packages:

```
catkin_make
source devel/setup.bash
```
Everything should now be built and ready for working on the assignment.


> *Sidenote:* At ITS, in room 307 there are two ubuntu (18.04) installations with ROS (Melodic), all necessary repositories cloned, and one robot connected to each machine. These setups can be used at group sessions.


To test our installation we can launch the robot in [Gazebo](http://gazebosim.org), which is the simulator we shall use.

```
roslaunch crustcrawler_gazebo empty_world.launch full_arm:=true tek4030:=true control:=effort
```

You should now see the robot standing in the middle of an empty world, awaiting creativity to flourish.

Enjoy!


## Delivery



<div align="center">
   <br>
  <img src="img\delivery.jpg"><br><br>
</div>


Make a repo called `TEK4030_Assignment_1_Simulation`, and a repo called `crustcrawler_controller` where the latter will be your ROS package. Both of these repositories shall be private repositories at [github.uio.no](github.uio.no), and make sure to add *eirikval* and *kimmat* as collaborators.

In order to do this assignment you will need to have knowledge equivalent of the following tutorials:

- [Navigating the File system](http://wiki.ros.org/ROS/Tutorials/NavigatingTheFilesystem)
- [Creating Package](http://wiki.ros.org/ROS/Tutorials/CreatingPackage)
- [Building Packages](http://wiki.ros.org/ROS/Tutorials/BuildingPackages)
- [Understanding Nodes](http://wiki.ros.org/ROS/Tutorials/UnderstandingNodes)
- [Understating Topics](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics)
- [Writing Publisher and Subscriber in C++](http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29)

The `TEK4030_Assignment_1_Simulation` repo shall contain at least a PDF with answers to the questions in the assignment, and a `README.md` containing a short summary and reflection on the assignment (how did the assignment go, what was easy, what was difficult, is there anything that could be improved?).

Upload all necessary files to these repos for delivery before the deadline.
