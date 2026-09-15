# ROS 2 CLI Cheat Sheet

Below we provide a non-exhaustive list of commonly used ROS2 commands, specifically the ones we cover in Chapters 1-3.

ROS 2 provides a unified CLI (`ros2`) with many subcommands for interacting with the system. See details in [\[docs.ros.org\]](https://docs.ros.org/en/jazzy/Concepts/Basic/About-Command-Line-Tools.html).

## Sourcing

The source commands are required if not included in the `.bashrc` file.

```bash
source /opt/ros/jazzy/setup.bash # Makes ROS 2 commands available
```

When you make changes to a workspace or open a new terminal, you also need to source it (if not included in the `.bashrc` file):

```bash
source ~/create3_ws/install/setup.bash # Activates the create3_ws workspace 
```

## Running

```bash
ros2 run create3_pkg simple_publisher # Runs the publisher node from the create3_pkg package
```

```bash
ros2 run turtlesim turtlesim_node # Starts TurtleSim
```

```bash
ros2 run turtlesim turtle_teleop_key # Starts keyboard control for TurtleSim
```

```bash
ros2 launch <package> <launch_file> # Starts nodes defined in a launch file
```

```bash
ros2 launch turtle_tf2_py turtle_tf2_demo.launch.py # Starts the TF2 TurtleSim demo
```

```bash
ros2 launch turtlesim_teleop_launch.xml # Runs the standalone TurtleSim launch file
```

```bash
ros2 run <pkg> <exec> --ros-args -r __node:=new_name # Runs a node with a new name
```

## Nodes

```bash
ros2 node list # Lists running ROS 2 nodes
```

```bash
ros2 node info <node_name> # Shows a node’s topics, services, and actions
```

## Topics

```bash
ros2 topic list # Lists active topics
```

```bash
ros2 topic list -t # Lists topics with their message types
```

```bash
ros2 topic echo <topic> # Prints messages published on a topic
```

```bash
ros2 topic info <topic> # Shows topic type and publisher/subscriber counts
```

```bash
ros2 topic hz <topic> # Gets the publishing rate of a topic
```

```bash
ros2 interface show <type> # Displays an interface’s structure
```

```bash
ros2 topic pub <topic> <type> "<data>" # Publishes a message <data> to a topic
```

## Services

```bash
ros2 service list # Lists available services
```

```bash
ros2 service list -t # Lists services with their types
```

```bash
ros2 service type <service> # Shows a service’s type
```

```bash
ros2 service call <service> <type> [arguments] # Calls a service
```

```bash
ros2 service info <service> # Displays information about a service
```

## Actions

```bash
ros2 action list # Lists available actions
```

```bash
ros2 action list -t # Lists actions with their types
```

```bash
ros2 action info <action> # Shows action clients and servers
```

```bash
ros2 action send_goal <action> <type> <values> # Sends an action goal
```

```bash
ros2 action send_goal ... --feedback # Sends a goal and displays feedback
```

## Bags

```bash
ros2 bag record <topic> # Records topic data
```

```bash
ros2 bag record <topic1> <topic2> # Records topic1 and topic2 data
```

```bash
ros2 bag record -o <name> <topic> # Records topic data with a chosen bag name
```

```bash
ros2 bag record -a -o <name> # Records all discovered topics
```

```bash
ros2 bag info <bag_name> # Displays bag metadata
```

```bash
ros2 bag play <bag_name> # Replays recorded topic data
```

## TFs

```bash
ros2 run tf2_tools view_frames # Generates a TF tree diagram
```

```bash
ros2 run tf2_ros tf2_echo <reference> <frame> # Displays the transform between frames
```

## Packages and Workspace

```bash
ros2 pkg list # List installed packages
```

```bash
ros2 pkg executables <pkg> # List executables in a package
```

```bash
ros2 pkg create create3_pkg --build-type ament_python --dependencies rclpy std_msgs # Creates a Python ROS 2 package
```

```bash
colcon build # Builds the workspace
```

```bash
colcon build --symlink-install # Builds the workspace with symbolic links
```

## Parameters

```bash
ros2 param list <node> # List all parameters
```

```bash
ros2 param get <node> <param> # Gets and displays a node’s parameter value
```

```bash
ros2 param set <node> <param> <value> # Changes a node’s parameter value
```

## Others

```bash
rqt # Opens the ROS graphical inspection tool
```

```bash
ros2 --help # Shows all ROS 2 commands
```

```bash
ros2 doctor # Checks ROS setup
```

## Navigation menu

- Go to [Chapter 1](../../Part_1-ROS/Chapter-1/readme.md)
- Go to [Chapter 2](../../Part_1-ROS/Chapter-2/readme.md)
- Go to [Chapter 3](../../Part_1-ROS/Chapter-3/readme.md)
- Go to [Chapter 4](../../Part_2-Create3/Chapter-4/readme.md)
- Go to [Chapter 5](../../Part_2-Create3/Chapter-5/readme.md)
- Go to [Chapter 6](../../Part_2-Create3/Chapter-6/readme.md)
- Go to the [Main page](../../readme.md)