# Tutorial 1: Exploring ROS2 framework

## Exploring ROS 2

Install the turtlesim package, if it's not already installed:

`sudo apt install ros-jazzy-turtlesim`

All binary ROS packages are all typically available on apt following the ros-<ros_version_name>-<package_name> convention.

Open a new terminal window and run the command:

`ros2 run turtlesim turtlesim_node`

The `ros2 run` command is the simplest way of running nodes. With the previous command we started the turtlesim_node which is located in the turtlesim package. In a third terminal, run the command:
`ros2 run turtlesim turtle_teleop_key`
This will allow you to control the turtle using the keyboard. Note that the terminal running the teleop requires the focus for the teleop to work.

Using the teleop node, messages are being sent to the turtlesim node. Open yet another terminal window and try to find out what's going on in the ROS system with the following commands:

- `ros2 topic`
- `ros2 interface`
- `ros2 service`
- `ros2 param`
- `ros2 doctor --report`
- `ros2 run rqt_graph rqt_graph`

Note that by typing −h or −help after the command verb will print information about the usage of these commands.

Run the node dis_tutorial1/draw_square.py, and observe its effect on the turtle. Print out and analyze the messages being sent to the turtle. Which node is responsible for the turtle movement and what is the structure of the messages?

Answer the following questions:

- Which **nodes** are currently active?
```bash
andrej@Zenbook-14X:~$ ros2 node list
/draw_square
/turtlesim
```
- What **topics** are currently active?
```bash
andrej@Zenbook-14X:~$ ros2 topic list
/parameter_events
/rosout
/turtle1/cmd_vel
/turtle1/color_sensor
/turtle1/pose
```
- What is the **message type** for each topic?
```bash
andrej@Zenbook-14X:~$ ros2 topic list -t
/parameter_events [rcl_interfaces/msg/ParameterEvent]
/rosout [rcl_interfaces/msg/Log]
/turtle1/cmd_vel [geometry_msgs/msg/Twist]
/turtle1/color_sensor [turtlesim/msg/Color]
/turtle1/pose [turtlesim/msg/Pose]
```
- To which topics is each node **publishing**?
```bash
andrej@Zenbook-14X:~$ ros2 node info /turtlesim
ros2 node info /draw_square
/turtlesim
  Subscribers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /turtle1/cmd_vel: geometry_msgs/msg/Twist
  Publishers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /rosout: rcl_interfaces/msg/Log
    /turtle1/color_sensor: turtlesim/msg/Color
    /turtle1/pose: turtlesim/msg/Pose
  Service Servers:
    /clear: std_srvs/srv/Empty
    /kill: turtlesim/srv/Kill
    /reset: std_srvs/srv/Empty
    /spawn: turtlesim/srv/Spawn
    /turtle1/set_pen: turtlesim/srv/SetPen
    /turtle1/teleport_absolute: turtlesim/srv/TeleportAbsolute
    /turtle1/teleport_relative: turtlesim/srv/TeleportRelative
    /turtlesim/describe_parameters: rcl_interfaces/srv/DescribeParameters
    /turtlesim/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
    /turtlesim/get_parameters: rcl_interfaces/srv/GetParameters
    /turtlesim/get_type_description: type_description_interfaces/srv/GetTypeDescription
    /turtlesim/list_parameters: rcl_interfaces/srv/ListParameters
    /turtlesim/set_parameters: rcl_interfaces/srv/SetParameters
    /turtlesim/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
  Service Clients:

  Action Servers:
    /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
  Action Clients:

/draw_square
  Subscribers:
    /turtle1/pose: turtlesim/msg/Pose
  Publishers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /rosout: rcl_interfaces/msg/Log
    /turtle1/cmd_vel: geometry_msgs/msg/Twist
  Service Servers:
    /draw_square/describe_parameters: rcl_interfaces/srv/DescribeParameters
    /draw_square/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
    /draw_square/get_parameters: rcl_interfaces/srv/GetParameters
    /draw_square/get_type_description: type_description_interfaces/srv/GetTypeDescription
    /draw_square/list_parameters: rcl_interfaces/srv/ListParameters
    /draw_square/set_parameters: rcl_interfaces/srv/SetParameters
    /draw_square/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
  Service Clients:
    /reset: std_srvs/srv/Empty
  Action Servers:

  Action Clients:
```
- To which topics is each node **subscribed**?
- What are the packages that define different **message types**?
```bash
andrej@Zenbook-14X:~$ ros2 pkg list | grep msgs
action_msgs
actionlib_msgs
diagnostic_msgs
geometry_msgs
lifecycle_msgs
map_msgs
nav_msgs
pcl_msgs
pendulum_msgs
rosgraph_msgs
sensor_msgs
sensor_msgs_py
service_msgs
shape_msgs
statistics_msgs
std_msgs
stereo_msgs
tf2_geometry_msgs
tf2_msgs
tf2_sensor_msgs
trajectory_msgs
unique_identifier_msgs
visualization_msgs
```
- Which **parameters** can be set on which nodes?
```bash
andrej@Zenbook-14X:~$ ros2 param list
/draw_square:
  start_type_description_service
  use_sim_time
/turtlesim:
  background_b
  background_g
  background_r
  holonomic
  qos_overrides./parameter_events.publisher.depth
  qos_overrides./parameter_events.publisher.durability
  qos_overrides./parameter_events.publisher.history
  qos_overrides./parameter_events.publisher.reliability
  start_type_description_service
  use_sim_time
```

Additionally, try to:
- Get a **visualization** of all the nodes and topics in the system.
`andrej@Zenbook-14X:~$ rqt_graph`
- Get a printout of all the **packages** installed in the system.
```bash
andrej@Zenbook-14X:~$ ros2 pkg list
action_msgs
action_tutorials_cpp
action_tutorials_interfaces
action_tutorials_py
actionlib_msgs
ament_cmake
ament_cmake_auto
ament_cmake_copyright
...
```
- Get a printout of all the **messages** installed in the system.
```bash
andrej@Zenbook-14X:~$ ros2 interface list | grep msg
    action_msgs/msg/GoalInfo
    action_msgs/msg/GoalStatus
    action_msgs/msg/GoalStatusArray
    actionlib_msgs/msg/GoalID
    actionlib_msgs/msg/GoalStatus
    actionlib_msgs/msg/GoalStatusArray
    builtin_interfaces/msg/Duration
    builtin_interfaces/msg/Time
    diagnostic_msgs/msg/DiagnosticArray
    diagnostic_msgs/msg/DiagnosticStatus
	...
```
- **Print** out the messages being published on each topic.
```bash
# Velocity commands from draw_square
ros2 topic echo /turtle1/cmd_vel

# Turtle position and angle
ros2 topic echo /turtle1/pose

# Color under turtle's pen
ros2 topic echo /turtle1/color_sensor
```
- **Publish** a message on each topic.
```bash
andrej@Zenbook-14X:~ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist \ \
  "{linear: {x: 1.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.0}}"
publisher: beginning loop
publishing #1: geometry_msgs.msg.Twist(linear=geometry_msgs.msg.Vector3(x=1.0, y=0.0, z=0.0), angular=geometry_msgs.msg.Vector3(x=0.0, y=0.0, z=1.0))

andrej@Zenbook-14X:~ros2 service call /turtle1/teleport_absolute turtlesim/srv/TeleportAbsolute \ \
  "{x: 5.0, y: 5.0, theta: 0.0}"
waiting for service to become available...
requester: making request: turtlesim.srv.TeleportAbsolute_Request(x=5.0, y=5.0, theta=0.0)

response:
turtlesim.srv.TeleportAbsolute_Response()
```

- **Set** the background color of turtlesim to a color of your choice.
```bash
andrej@Zenbook-14X:~$ ros2 param set /turtlesim background_r 0
ros2 param set /turtlesim background_g 0
ros2 param set /turtlesim background_b 139
Set parameter successful
Set parameter successful
Set parameter successful
```


Explore the usage of other commands that are found in the [ROS 2 Cheatsheet](https://www.theconstructsim.com/wp-content/uploads/2021/10/ROS2-Command-Cheat-Sheets-updated.pdf). You can also find the full turtlesim documentation [here](https://docs.ros.org/en/jazzy/Tutorials/Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.html#prerequisites).

## Writing a package

Write a package that contains a simple program that you can run using the `ros2 run` command and outputs a string (e.g. "Hello from ROS!") to the terminal every second.

Use the following tutorials as a starting point:

- [Creating a package](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html)

Now we can add two more nodes, one that sends a message and another one that retrieves it and prints it to the terminal.

- [Simple publisher and subscriber in C++](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.html)
- [Simple publisher and subscriber in Python](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Publisher-And-Subscriber.html)

## Building a package

In order to build a custom package, you should move to the workspace root directory and run the following: `colcon build`. This will build all the packages within the `src` subdirectory. Since this could take a long time, the flag `--packages-select <package name>` can be used to only build selected packages. The build process will also create setup files in the `install` subdirectory. To make the newly built packages visible to ROS2, you should run the following: `source install/local_setup.bash`. Then, you will be able to run your custom nodes using the following command: `ros2 run <package_name> <node_name>`.

## Services

In the tutorial you have examples of creating a service and a client as well as defining a custom service interface. We define a custom service by specifying the structure of the request that the service will accept and the response that it will return. 

Use the following tutorials as a starting point:

- [Simple service and client in C++](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Cpp-Service-And-Client.html)
- [Simple publisher and subscriber in Python](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Service-And-Client.html)

See the [interfaces doc](https://docs.ros.org/en/jazzy/Concepts/Basic/About-Interfaces.html) for a reference of all the possible native data types. The `ros2 interfaces list` command shows all built messages, services, and actions, which are themselves types that can be used in custom interfaces.

Note that only `ament_cmake` packages are capable of building interfaces (i.e. services and messages), so if you have an `ament_python` package you'll need a separate one for message definitions.

------

### 🗎 Tips and tricks

For more useful code snippets, check out the [ROS 2 Cookbook](https://github.com/mikeferguson/ros2_cookbook).

Since running several different nodes in parallel requires multiple terminals, we recommend you to use a terminal emulator that is able to display several terminals in a single window. A good choice is [Terminator](https://gnome-terminator.org/). You can install it with `sudo apt install terminator`. It should also be preinstalled on the classroom computers. You can split the screen horizontally or verically with the shortcuts `ctrl + shift + e` and `ctrl + shift + o`, respectively.

In order for your packages to be visible to ROS2, you will need to run `source install/setup.bash` in your workspace directory after build. As this holds for all terminals, a good idea is to add the following to your `.bashrc` file: `source /home/<user>/ros2_workspace/install/local_setup.bash`

Running `colcon build` is necessary after any source code changes, but only for C++ nodes (or messages/services). If writing python nodes, and you build the package using the flag `--symlink-install`, the links to your python source files were created and changing your code should work without building the package again. If you run into any build errors that don't make any sense, try deleting the `build`, `log` and `install` directories and run the build again.

Here are some other useful colcon parameters:
- `--cmake-args=-DCMAKE_BUILD_TYPE=Release` (disable debugging, enable compile time optimization)
- `--executor sequential` (use single threaded compilation, takes longer but uses less memory which can be useful when compiling large projects on limited hardware)

When in doubt, reset the cache server: `ros2 daemon stop; ros2 daemon start`

Occasionally, it can be easier to debug new topics and services by publishing or calling straight from the command line, instead of from a node. Following are some commands that can help you with this:
```
# send message from terminal
ros2 topic pub <topic name> <message type> <data> -r <publishing rate>

# e.g.:
ros2 topic pub /chat std_msgs/String "data: test" -r 1


# find interface for service
ros2 interface show <service interface name>

e.g.:
ros2 interface show dis_tutorial1/srv/AddTwoInts


# call service from terminal
ros2 service call <service name> <service interface> <data in yaml format>

# e.g.: (note that spaces in data field are important)
ros2 service call add_two_ints dis_tutorial1/srv/AddTwoInts "{a: 1, b: 2}"
```

# Homework 1

## Part 1

Open the file `homework1_answers.txt` from this repository, follow the instructions and include your answers to the questions in the marked slots in the same file. Then, upload the file on the available link on Učilnica.

## Part 2

After you have installed and tested ROS2, as well as set up your own workspace you should:

- Create a new package. In the package you can use C++ or Python.
- Create a custom message type, that has a string fields, an integer field, and a bool field.
- Create a custom service type, where the request contains a string field and an array of integers, and the response contains a string field and an integer field.
- Create a publisher node, that periodically sends a message on a certain topic. You should use the custom message you defined.
- Create a subscriber node, that recieves the message on the same topic and prints out its contents.
- Create a custom service node that accepts an array of integers and responds with the SUM of the recieved integers. Use the custom service you defined.
- Create a custom client node that generates random sequences of 10 integers, calls the service node, and prints out the response that it recieves from your service node.

Compress your package into a single .zip archive named `homework1_package.zip` and upload the file on the available link on Učilnica.