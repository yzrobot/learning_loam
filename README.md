# TO52 - SLAM with a 3D lidar

## ROS : How it works

http://wiki.ros.org/ROS/Concepts

- loam_velodyne.launch will :
	- start the master (allow the nodes to find each other, exchange messages and use services)
	- start the nodes (processes) written in cpp (or py) and compiled using gcc + rviz (a node)
		(exemple for a robot : 1 node laser range-finder, 1 node controls the wheel motors, 1 node performs localization, 1 node performs path planning, 1 Node provides a graphical view of the system ...)

- the node, after reporting their registrations to the master, are able to speak directly together by sending messages (request or response) to a subscribed topic ( as the services are stored in the master), it uses TCPROS (based on TCP/IP socket)



## How to build and run your catkin_workspace without Qt

- in your repo write cmd "catkin_make" in order to build the nodes (using your cpp files)
- launch 3 terminals, give their the source " source {...}devel/setup.bash"
- in one : "roslaunch loam_velodyne loam_velodyne.launch" (will launch all the nodes needed)
- in an other : "roslaunch loam_velodyne loam_velodyne.launch" (will convert the lidar data into PointcloudMessage)
- and finally : "rosbag play {your bag file}"



## How to build and run your catkin_workspace using Qt

- !!! Do not install Qt !!!
- Follow this tutorial first : https://ros-industrial.github.io/ros_qtc_plugin/_source/How-to-Install-Users.html
- Open Qt, create new project by using "other application" and selecting "ros industrial" and your dedicated workspace
- manage you compilation and run tool chain (click on project on the left)
- in run, you may have :
    - 2 launch step :
        - one with package loam_volodyne and loam_velodyne launch as target (will launch all the nodes needed)
        - the other velodyne_pointcloud and 32e_points.launch (will convert the lidar data into PointcloudMessage)
    - a run step : 
        - with package rosbag target play and argument your bag file


## How to debug using Qt (currently not working)

To debug only one object could be debug (one node) : 
- Insert your breakpoint
- Menu Bar > Debug > Start Debugging > Attach to Unstarted Application...
- Browse to the executable (devel/lib/loam_velodyne/multiScanRegistration) by exemple
- select start watching
- run your project (Ctrl + R)
- depending on where the breakpoints it should be stop when it reaches one 

Romain Henry, Florent Willemin, Zhi Yan, and Yassine Ruichek
