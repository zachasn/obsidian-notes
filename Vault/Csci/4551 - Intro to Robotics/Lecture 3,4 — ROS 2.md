# ROS
- ROS is not a "true" OS → it's peer-to-peer *middleware* package that sits between OS and robot hardware
- Allows for easier hardware abstraction and code reuse
- All major functionality is broken up into a number of chunks that communicate with each other using messages
## Key Concepts
- **Node:** A program/process that is running, each node should do one thing well → Linux philosophy 
- **Publisher:** A node that send messages to a specific topic
- **Subscriber:** A node that receives messages from a specified topic
## Why ROS?
- **Benefits:**
	- Proven in use across multi domains and multi platforms
	- Easily adaptable to new ones
	- Makes code portable across robots
	- Make code reusable → stops reinvention of code
	- Large user base → encourages code sharing, dissemination of research results, improves verifiability
	- Supported by hardware vendors and the Open Source Robotics Foundation (OSRF)
- Open-sourced, well-supported and documented
## ROS 2 Software Ecosystem
- **Middleware:** encompasses communication among components, from network APIs to message parsers
- **Algorithms:** Provides many of the algorithms commonly used when building robotics applications, *e.g.*, perception, SLAM, planning, etc in a robotic-agnostic fashion
- **Dev tools:** suite of cmd line and graphical tools for configuration, launch, introspection, visualization, debugging, simulation, and logging
## Communication (data passing) in ROS 2
![[data_passing.png]]
1. **Messages:**  *Publisher*/*Subscribers* are used to share messages via specified topic
2. **Services:**  Request/Reply model for *remote procedure calls* → one request/reply is mode then done
3. **Actions:** Continuous feedback request/reply model for long-running tasks
4. **Parameters:** Modify global data using a *request/reply* pattern
5. *DDS* handles communication setup
## Components: 
1. **Topics**
	- A method for transferring data between nodes,  enabling communication across different parts of the system
	- Represented as /topic_name, and have a message type
	- Can create custom message types and have the topic send/receive that message
	- Topics don't have to only be point-to-point communication → it can be one-to-many, many-to-one, or many-to-many
	- *DDS* handles communication setup
2. **Services**
	- Communication for nodes based on a *call-and-response-model*
	- Topics allow nodes to subscribe to data streams and get continual updates
	-  Services only provide data when that are specifically called by a client
	-  There can be many service clients using the service, but there can only be one service server for a service
3. **Actions**
	- Communication for long-running tasks with three parts: a goal, feedback, and a result
	- Built on topics and services
	- Uses a client-server model similar to services
	- Key difference: actions can be canceled and provide continuous feedback
	- Ideal for tasks that take time to complete (e.g., navigation, manipulation)
	- Action clients can cancel goals and receive periodic status updates
4. **Parameters**
	- Configuration/setting values of nodes
	- Each node maintains its own parameters
		- *e.g.*, a camera node can store camera frame rate, resolution, etc as parameters
	- some **very useful** ROS 2 command line:
		- ros2 topic: list, echo, info, type, pub, hz, bw
		- ros2 node: list, info
		- ros2 service: list, call, type
		- ros2 param: list, get, set, dump
		- ros2 pkg, launch, daemon, interface
		- ros2 pkg executables (pkg name): get pkg executables
## ROS 2 Tools

### Development and Debugging Tools
- **rqt_graph:** Visualizes the ROS computation graph showing nodes, topics, and connections
	- `ros2 run rqt_graph rqt_graph` (exact command is version dependent)
- **rqt_reconfigure:** GUI for dynamically modifying node parameters at runtime
	- `ros2 run rqt_reconfigure rqt_reconfigure`
- **rviz2:** 3D visualization tool for displaying sensor data, robot models, and planning results
	- `ros2 run rviz2 rviz2`

### Hardware Interface Tools
- **v4l2_camera:** Video for Linux camera driver node (hardware-agnostic)
	- `ros2 run v4l2_camera v4l2_camera_node`
- **usb_cam:** Alternative USB camera driver
	- `ros2 run usb_cam usb_cam_node_exe`

### Simulation Tools
- **Gazebo:** Physics-based 3D simulator for robotics
- **TurtleSim:** Simple 2D simulator for learning ROS concepts
	- `ros2 run turtlesim turtlesim_node`

## ROS 2 Package System

### Workspaces and Build System
- **Workspace:** A directory containing ROS 2 packages, typically organized as:
	```
	workspace/
	├── src/           # Source code for packages
	├── build/         # Build artifacts
	├── install/       # Installed packages
	└── log/           # Build logs
	```

### Overlays and Underlays
- **Underlay:** The base ROS 2 installation providing core functionality
- **Overlay:** A workspace built on top of an underlay, allowing you to:
	- Add custom packages
	- Override existing packages
	- Create isolated development environments
- **Sourcing:** Must source the workspace setup script to use packages
	- `source install/setup.bash` (for bash)
### Package Management
- **Creating packages:** `ros2 pkg create <package_name>`
- **Building workspace:** `colcon build` (in workspace root)
- **Package dependencies:** Defined in `package.xml`
- **Build configuration:** Defined in `CMakeLists.txt` (C++) or `setup.py` (Python)

## ROS 2 Architecture & Launch System

### ROS 2 Architecture
- **Distributed System:** Nodes can run on different machines and communicate over a network
- **DDS Middleware:** Data Distribution Service handles low-level communication
	- Provides discovery, serialization, and transport
	- Supports Quality of Service (QoS) policies for reliable/best-effort delivery
- **Executors:** Manage callback execution and threading within nodes
- **Lifecycle Nodes:** Nodes with managed states (inactive, active, finalized)

### Launch Files
Launch files automate the startup of multiple nodes and configure the system:

- **Purpose:** Start multiple nodes with specific configurations in a coordinated manner
- **File formats:** Python (.py)
- **Key features:**
	- Launch multiple nodes simultaneously
	- Set parameters
	- Configure *namespaces* in a single command
**Example launch file structure:**
```python
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
    return LaunchDescription([
        Node(
            package='turtlesim',
            executable='turtlesim_node’,
            name='custom_node_name',
            #parameters=[{'background_r': '255'}],
            name='sim1'
        ),
        Node(
	        package='turtlesim',
            executable='turtle_teleop_key',
            name='teleop',
            #parameters=[{'background_r': '255'}],
            prefix='xterm -e' # opens in a new terminal
        )
    ])
```
**Launch file does the following:**
1. Launches the`turtlesim`node and the`turtle_teleop_key`node
2. Specifies the following for both nodes:
	1. The`package`that contains the node
	2. the node`executable`file
	3. the`name`of the node
3. for the`turtle_teleop_key`, the launch file specifies that a new terminal should be created for starting that node by using`prefix='xterm -e'`
**Running launch files:**
- `ros2 launch <package_name> <launch_file>`
- `ros2 launch <path_to_launch_file>`
