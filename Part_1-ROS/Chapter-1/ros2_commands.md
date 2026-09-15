# Common ROS 2 Commands (ROS 2 CLI Cheat Sheet)

ROS 2 provides a unified CLI (`ros2`) with many subcommands for interacting with the system. See details in [\[docs.ros.org\]](https://docs.ros.org/en/jazzy/Concepts/Basic/About-Command-Line-Tools.html). Below we provide a non-exhaustive list of commonly used ROS2 commands, specifically the ones we cover in Chapters 1-3.

## Sourcing

- `source /opt/ros/jazzy/setup.bash` - Sources ROS 2 to make the commands available (required if not included in your .bashrc).
- `source ~/create3_ws/install/setup.bash` - Activates the create3_ws workspace.

## Running

- `ros2 run create3_pkg simple_publisher` - Runs the publisher node from the create3_pkg package.
- `ros2 run turtlesim turtlesim_node` - Starts TurtleSim.
- `ros2 run turtlesim turtle_teleop_key` - Starts keyboard control for TurtleSim.
- `ros2 launch <package> <launch_file>` - Starts nodes defined in a launch file.
- `ros2 launch turtle_tf2_py turtle_tf2_demo.launch.py` - Starts the TF2 TurtleSim demo.
- `ros2 launch turtlesim_teleop_launch.xml` - Runs the standalone TurtleSim launch file.
- `ros2 run <pkg> <exec> --ros-args -r __node:=new_name` - Runs a node with a new name.

## Nodes

- `ros2 node list` - Lists running ROS 2 nodes.
- `ros2 node info <node_name>` - Shows a node’s topics, services, and actions.

## Topics

- `ros2 topic list` - Lists active topics.
- `ros2 topic list -t` - Lists topics with their message types.
- `ros2 topic echo <topic>` - Prints messages published on a topic.
- `ros2 topic info <topic>` - Shows topic type and publisher/subscriber counts.
- `ros2 topic hz <topic>` - Gets the publishing rate of a topic.
- `ros2 interface show <type>` - Displays an interface’s structure.
- `ros2 topic pub <topic> <type> "<data>"` - Publishes a message <data> to a topic.

## Services

- `ros2 service list` - Lists available services.
- `ros2 service list -t` - Lists services with their types.
- `ros2 service type <service>` - Shows a service’s type.
- `ros2 service call <service> <type> [arguments]` - Calls a service.
- `ros2 service info <service>` - Displays information about a service.

## Actions

- `ros2 action list` - Lists available actions.
- `ros2 action list -t` - Lists actions with their types.
- `ros2 action info <action>` - Shows action clients and servers.
- `ros2 action send_goal <action> <type> <values>` - Sends an action goal.
- `ros2 action send_goal ... --feedback` - Sends a goal and displays feedback.

## Bags

- `ros2 bag record <topic>` - Records topic data.
- `ros2 bag record <topic1> <topic2>` - Records topic1 and topic2 data.
- `ros2 bag record -o <name> <topic>` - Records topic data with a chosen bag name.
- `ros2 bag record -a -o <name>` - Records all discovered topics.
- `ros2 bag info <bag_name>` - Displays bag metadata.
- `ros2 bag play <bag_name>` - Replays recorded topic data.

## TFs

- `ros2 run tf2_tools view_frames` - Generates a TF tree diagram.
- `ros2 run tf2_ros tf2_echo <reference> <frame>` - Displays the transform between frames.

## Packages and Workspace

- `ros2 pkg list` - List installed packages
- `ros2 pkg executables <pkg>` - List executables in a package
- `ros2 pkg create create3_pkg --build-type ament_python --dependencies rclpy std_msgs` - Creates a Python ROS 2 package.
- `colcon build` - Builds the workspace.
- `colcon build --symlink-install` - Builds the workspace with symbolic links.

## Parameters

- `ros2 param list <node>` - List all parameters.
- `ros2 param get <node> <param>` - Gets and displays a node’s parameter value.
- `ros2 param set <node> <param> <value>` - Changes a node’s parameter value.

## Others

- `rqt` - Opens the ROS graphical inspection tool.
- `ros2 --help` - Show all ROS 2 commands.
- `ros2 doctor` - Check ROS setup.
