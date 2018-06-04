# SLAM with a 3D lidar

## 2018 UTBM TO52 project

### Romain Henry, Florent Willemin, Zhi Yan, and Yassine Ruichek

# ROS : How it works

http://wiki.ros.org/ROS/Concepts

- loam_velodyne.launch will :
	- start the master (allow the nodes to find each other, exchange messages and use services)
	- start the nodes (processes) written in cpp (or py) and compiled using gcc + rviz (a node)
		(exemple for a robot : 1 node laser range-finder, 1 node controls the wheel motors, 1 node performs localization, 1 node performs path planning, 1 Node provides a graphical view of the system ...)

- the node, after reporting their registrations to the master, are able to speak directly together by sending messages (request or response) to a subscribed topic ( as the services are stored in the master), it uses TCPROS (based on TCP/IP socket)


## How to build and run your catkin_workspace
### Using command line

- in your repo write cmd "catkin_make" in order to build the nodes (using your cpp files)
- launch 3 terminals, give their the source " source {...}devel/setup.bash"
- in one : "roslaunch loam_velodyne loam_velodyne.launch" (will launch all the nodes needed)
- in an other : "roslaunch loam_velodyne loam_velodyne.launch" (will convert the lidar data into PointcloudMessage)
- and finally : "rosbag play {your bag file}"



### Using Qt and ROS Industrial plugin

- !!! Don't install Qt !!!
- Follow this tutorial first : https://ros-industrial.github.io/ros_qtc_plugin/_source/How-to-Install-Users.html
- Open Qt, create new project by using "other project" and selecting "ros workspace" and your dedicated workspace
- manage your compilation and run tool chain (click on projects on the left)
- in run, you may have :
    - 2 launch step :
        - one with package loam_volodyne and loam_velodyne launch as target (will launch all the nodes needed)
        - the other velodyne_pointcloud and 32e_points.launch (will convert the lidar data into PointcloudMessage)
    - a run step : 
        - with package rosbag target play and argument your bag file


## How to debug
### Using GDB command line

One way to debug :
GDB : 
- Add prefix : "launch-prefix="xterm -e gdb --args" -> run your node in a gdb in a separate xterm window, manually type run to start it

Example for debugging node LaserOdometry : edit loam_velodyne.launch
- Edit file and add prefix :
	<node pkg="loam_velodyne" type="laserOdometry" name="laserOdometry" output="screen" respawn="true"  **launch-prefix="xterm -e gdb --args"** >
	    <param name="scanPeriod" value="$(arg scanPeriod)" />
	 </node>
- Compile & run in normal mode
- A new gdb windows opens. You can use gdb tools (add breakpoint, display variable..)
- Write run

### Using Valgrind 

use valgrind just for finding memory leaks and array overflows
to enable Valgring, use the same way as gdb. Just change prefix : launch-prefix="valgrind"

for more informations using gdb or Valgrind : http://wiki.ros.org/roslaunch/Tutorials/Roslaunch%20Nodes%20in%20Valgrind%20or%20GDB

### Using Qt

To debug, only one object (one node) could be debug  : 
- Don't forget to allow ptrace ! by following these instructions :
	- Open file: sudo gedit /etc/rc.local
	- Add this line before the exit 0 line: echo 0 | tee /proc/sys/kernel/yama/ptrace_scope
- Insert your breakpoint
- Menu Bar > Debug > Start Debugging > Attach to Unstarted Application...
- Browse to the executable (devel/lib/loam_velodyne/multiScanRegistration) by exemple
- select start watching
- run your project (Ctrl + R)
- depending on where the breakpoints it should be stop when it reaches one 

### Using RQT command line
- rqt_graph will show us interactions between the nodes on the ros system
	- The rectangles in the the window show the topics currently available on the system.
	- The ovals are ROS nodes. Arrows leaving the node indicate the topics the node publishes, and arrows entering the node indicate the topics the node subscribes to.
