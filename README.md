# Multi-Agent-Mapping
This package is designed to evaluate different exploration strategies for creating a shared map of a larger environment. Currently, the programs are ran on three turtlebots each running `GMapping`, with the task of mapping the `turtlebot_house`. This repository is a constant work-in-progress so expect many changes to occur over time.

## System Requirements
Before cloning this repository, make sure that you have [ROS Melodic](http://wiki.ros.org/melodic/Installation/Ubuntu#Installation) installed on your system. At the time of this commit, ROS Melodic is currently targeted at Ubuntu 18.04, but may run on a Windows or Mac OS system. Additionally, it may be possible to run this package inside of a Docker Container based on Ubuntu 18.04 with ROS Melodic installed. However, please note that certain launch files rely upon access to an X-Server, which may require additional configuration if using a Docker container.

### Dependency Notes
Before installing this package, you will also need to manually install the `multirobot_map_merge` and `explore_lite` nodes as they do not share a presence in any apt repositories. To do this, simply clone the repositories into your catkin workspace and run `catkin_make`. You may also need to run `rosdep` in order for your dependencies for those packages to be properly configured.

```bash
# Make sure you are in the source directory of your catkin workspace.
git clone https://github.com/hrnr/m-explore/tree/melodic-devel
rosdep install explore_lite
rosdep install multirobot_map_merge
cd ..
catkin_make
```

## Installation Instructions
To install this package, simply clone the repository into your catkin workspace and run `catkin_make`. The `package.xml` file should have the dependencies already listed and will install those from the appropriate `apt` repositories.

```bash
# Make sure you are in the source directory of your catkin workspace.
git clone https://github.com/JDaniel29/Multi-Agent-Mapping-Work
rosdep install multi-agent-mapping
cd ..
catkin_make
```
# Launch File Overview
There are three main launch files that should help you get started in testing the packages I used for the Summer CI. They are the `multi-agent-mapping.launch`, `explore-lite.launch` and `rosbag_map_merge_test.launch` files. The first launch file will start the `turtlebot3_house` Gazebo simulation which includes three turtlebots stationed in a house. It will also start SLAM nodes along with navigation stacks for each robot. Lastly, it will also start the `multirobot_map_merge` node. Feel free to tweak the parameters used for the merging node, as the performance on your system may change based on how frequently it merges the maps together.

The `explore-lite.launch` file immediately launches the `explore-lite` nodes for each of the turtlebots. Once again, feel free to tweak the settings based on your needs.

The `rosbag_map_merge_test.launch` file is useful for testing the `multirobot_map_merge` package based on already existing scans. If you run that launch file, then you can use `rosbag` to replay any previous explorations and see how different settings influence the performance of the merging node. If you choose to use this node, make sure that you have specifically recorded all `/map` messages from the individual robots. The launch file is also configured to work with three robots explicitly, so you may need to change the number of robot namespaces for the transform messages if you are testing a different number of robot maps. Lastly, the node is configured to work without knowledge of the original odometry positions of the robots. If you wish to change that setting, you will also need to record `/odom` messages and input those into the parameters that have been set in the launch file. You should check the [ROS Wiki page](http://wiki.ros.org/multirobot_map_merge) for the `multirobot_map_merge` node for more information on that setting.

## Using RViz

If you wish to use RViz while testing the concepts used in this package, I have two configuration files included that can help visualize the progress made in creating a complete map. They are located in the `rviz` directory, and can be launched by running these two commands in different terminals:
```bash
rosrun rviz rviz -d rviz/multi-agent-mapping.rviz
rosrun rviz rviz -d rviz/multi-agent-mapping-2.rviz
```

# Credits and Acknowledgments

This package mainly takes advantage of Dr. Hörner's `multirobot_map_merge` package that he developed for his bachelor's thesis. More information about his package can be found [here](http://wiki.ros.org/multirobot_map_merge#Acknowledgements).

```
Hörner, J. (2016). Map-merging for multi-robot system. Charles University in Prague, Faculty of Mathematics and Physics).
```
