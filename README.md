# Learning LOAM (Course Code: TO52)

by Romain Henry and Florent Willemin

## How to get a LOAM full map?
The first solution we found out is just to comment out ```_laserCloudSurround->clear();``` in ```LaserMapping.cpp```. However, this trick will fill up the RAM since it just doesn't clear the data structures of the LOAM algorithm. For more details, please refer to our report [TO52_2018_Henry_Willemin.pdf](TO52_2018_Henry_Willemin.pdf).

<img src="https://github.com/epan-utbm/TO52/blob/2018_Henry_Willemin/images/loam_fullmap.jpg" align="middle" width="360"/> 

## So what about using OctoMap?

[loam_velodyne_octomap.launch](loam_velodyne/launch/loam_velodyne_octomap.launch)

<img src="https://github.com/epan-utbm/TO52/blob/2018_Henry_Willemin/images/octomap_building.gif" align="middle"/>
<img src="https://github.com/epan-utbm/TO52/blob/2018_Henry_Willemin/images/octomap_fullmap.png" align="middle" width="360"/>

## Our first experience with two HDL-32E LiDAR data:

[loam_velodyne_two.launch](loam_velodyne/launch/loam_velodyne_two.launch)

<img src="https://github.com/epan-utbm/TO52/blob/2018_Henry_Willemin/images/tow_velodynes.gif" align="middle"/>

## How to build and run the project with Qt

- Select the branch that is interesting to you
- !!! Don't install Qt !!!
- Follow this tutorial first : https://ros-industrial.github.io/ros_qtc_plugin/_source/How-to-Install-Users.html
- Open Qt, create new project by using "other project" and selecting "ros workspace" and your dedicated workspace
- Manage your compilation and run tool chain (click on projects on the left)
- In run, you may have one launch step with package loam_velodyne and [loam_velodyne.launch] {depend of your branch} as target.

## ROS

See : http://wiki.ros.org/ROS/Concepts to get informations about it.

- the ```loam_velodyne_xxx.launch``` will :
	- start the master (allow the nodes to find each other, exchange messages and use services)
	- start the nodes (processes) written in cpp (or py) and compiled using gcc + rviz (a nodes)
	- start the the LiDAR record (we used a ".bag")
	- start a node that will translate the content of the ".bag" in pointclouds

## How to debug with Qt

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
