# sawRobotIO1394ROS2

ROS 2 node for sawRobotIO1394 (https://github.com/jhu-saw/sawRobotIO1394).

You can use the ROS 2 VCS python-based tool (`sudo apt install python3-vcstool`) to download all the cisst, sawRobotIO1394 and ROS 2 specific packages needed for the sawRobotIO1394 ROS 2 node.  Look for the `.repos` file in this repository and find the link to the raw content.  Then in your ROS 2 workspace, under the `src` directory, use:
```sh
vcs import --input https://raw.githubusercontent.com/jhu-saw/sawRobotIO1394ROS2/main/robot_io.vcs
```
Then go back to the root of your ROS 2 workspace and build using:
```sh
colcon build
```

Once the code is compiled, you can start the ROS 2 NDI tracker using:
```
ros2 run robot_io robot_io
```
