## 📋 Table of Contents

1. [Chapter 1 - Sourcing ROS](#chapter-1-sourcing-ros)
2. [Chapter 2 - ROS Executables from Packages](#chapter-2-ros-executables-from-packages)
3. [Chapter 3 - ROS Nodes](#chapter-3-ros-nodes)
4. [Chapter 4 - ROS Topics](#chapter-4-ros-topics)
5. [Chapter 5 - ROS Services](#chapter-5-ros-services)
6. [Chapter 6 - ROS Parameters](#chapter-6-ros-parameters)
7. [Chapter 7 - ROS Actions](#chapter-7-ros-actions)
8. [Chapter 8 - ROS Workspace](#chapter-8-ros-workspace)

---

## Chapter 1 - Sourcing ROS

### 📌 What is Sourcing?

**Sourcing** is the process of activating the ROS2 environment in your terminal.

### 🔧 Setup Methods

#### Method 1: Temporary (One-time)

```bash
source /opt/ros/humble/setup.bash
```

This only works for the current terminal session.

#### Method 2: Permanent

```bash
# Add to .bashrc
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc

# Apply changes
source ~/.bashrc
```

#### Method 3: For Workspace

```bash
# Source your workspace
source ~/ros2_ws/install/setup.bash
```

### ✅ Verify Setup

```bash
# Check ROS2 version
ros2 --version

# List ROS2 packages
ros2 pkg list

# Check ROS distribution
echo $ROS_DISTRO
```

**Result:** If it shows `humble`, you're good! ✅

---

## Chapter 2 - ROS Executables from Packages

### 📦 What is a Package?

A **package** is a unit that organizes ROS2 code.

### 🔍 Finding Packages

```bash
# List all packages
ros2 pkg list

# Count packages
ros2 pkg list | wc -l

# Find specific package
ros2 pkg list | grep turtlesim
```

### 🏃 Running Executables

```bash
# Run package executable
ros2 run <package_name> <executable_name>

# Example: Run turtlesim node
ros2 run turtlesim turtlesim_node

# Example: Run teleop
ros2 run turtlesim turtle_teleop_key
```

### 📋 Package Information

```bash
# Describe package
ros2 pkg describe <package_name>

# List package executables
ros2 pkg executables <package_name>

# Find package location
ros2 pkg prefix <package_name>
```

---

## Chapter 3 - ROS Nodes

### 🎯 What is a Node?

A **Node** is a single executable unit in ROS2.

```
┌─────────────┐
│   Node 1    │ ← One task
├─────────────┤
│   Node 2    │ ← One task
├─────────────┤
│   Node 3    │ ← One task
└─────────────┘
```

### 🔍 Managing Nodes

```bash
# List running nodes
ros2 node list

# Get node info
ros2 node info <node_name>

# Example:
ros2 node info /turtle1
```

### 📊 Node Information Example

```bash
ros2 node info /turtle1

# Result:
/turtle1
  Subscribers:
    /turtle1/cmd_vel: geometry_msgs/msg/Twist
  Publishers:
    /turtle1/pose: turtlesim/msg/Pose
    /turtle1/color_sensor: turtlesim/msg/Color
```

### 🛠️ Remapping Nodes

```bash
# Rename node
ros2 run turtlesim turtlesim_node --ros-args --remap __node:=my_turtle

# Remap topic
ros2 run turtlesim turtlesim_node --ros-args --remap /turtle1/cmd_vel:=/my_cmd
```

---

## Chapter 4 - ROS Topics

### 📡 What is a Topic?

A **Topic** is how nodes communicate with each other.

```
┌─────────────┐      Topic       ┌─────────────┐
│  Publisher  │ ───────────────→ │  Subscriber │
│  Node       │   /turtle1/cmd_vel│  Node       │
└─────────────┘                  └─────────────┘
```

### 🔍 Managing Topics

```bash
# List all topics
ros2 topic list

# Get topic info
ros2 topic info <topic_name>

# Example:
ros2 topic info /turtle1/cmd_vel

# Get message type
ros2 topic type <topic_name>

# Example:
ros2 topic type /turtle1/cmd_vel
# Result: geometry_msgs/msg/Twist
```

### 📝 Viewing Messages

```bash
# Echo topic messages
ros2 topic echo <topic_name>

# Example:
ros2 topic echo /turtle1/pose

# Echo once
ros2 topic echo /turtle1/pose --once

# Echo with timeout
ros2 topic echo /turtle1/pose --timeout 5
```

### 📤 Publishing Messages

```bash
# Publish to topic
ros2 topic pub <topic_name> <msg_type> <data>

# Example: Move turtle forward
ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0}, angular: {z: 0.0}}"

# Publish continuously (1 Hz)
ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0}, angular: {z: 0.0}}" -r 1
```

### 📊 Topic Statistics

```bash
# Check bandwidth
ros2 topic bw <topic_name>

# Check message rate
ros2 topic hz <topic_name>
```

---

## Chapter 5 - ROS Services

### 🔧 What is a Service?

**Service** is request/response communication.

```
┌─────────────┐    Request    ┌─────────────┐
│   Client    │ ────────────→ │   Server    │
│             │               │             │
│   Response  │ ←──────────── │             │
└─────────────┘    Response   └─────────────┘
```

### 🔍 Managing Services

```bash
# List all services
ros2 service list

# Get service type
ros2 service type <service_name>

# Example:
ros2 service type /spawn

# Result: turtlesim/srv/Spawn
```

### 📞 Calling a Service

```bash
# Call service
ros2 service call <service_name> <service_type> <data>

# Example: Spawn new turtle
ros2 service call /spawn turtlesim/srv/Spawn "{x: 2.0, y: 2.0, theta: 0.0, name: 'turtle2'}"

# Result:
name: 'turtle2'
```

### 📋 Service Types

| Service | Type | Use |
|---------|------|-----|
| `/spawn` | turtlesim/srv/Spawn | Create new turtle |
| `/kill` | turtlesim/srv/Kill | Delete turtle |
| `/clear` | std_srvs/srv/Empty | Clear screen |
| `/reset` | std_srvs/srv/Empty | Reset |
| `/turtle1/set_pen` | turtlesim/srv/SetPen | Change pen color |
| `/turtle1/teleport_absolute` | turtlesim/srv/TeleportAbsolute | Teleport turtle |

---

## Chapter 6 - ROS Parameters

### ⚙️ What is a Parameter?

**Parameters** are configuration values for nodes.

### 🔍 Viewing Parameters

```bash
# List all parameters
ros2 param list

# List node parameters
ros2 param list <node_name>

# Example:
ros2 param list /turtle1

# Get parameter value
ros2 param get <node_name> <param_name>

# Example:
ros2 param get /turtle1 background_r
```

### ✏️ Setting Parameters

```bash
# Set parameter
ros2 param set <node_name> <param_name> <value>

# Example: Change background to red
ros2 param set /turtlesim background_r 255
ros2 param set /turtlesim background_g 0
ros2 param set /turtlesim background_b 0

# Result: Background becomes red
```

### 📝 Parameter Files (YAML)

**File: params.yaml**
```yaml
/turtlesim:
  ros__parameters:
    background_r: 100
    background_g: 150
    background_b: 200
```

**Load parameter file:**
```bash
ros2 run turtlesim turtlesim_node --ros-args --params-file params.yaml
```

### 🛠️ Parameter Commands

| Command | Description |
|---------|-------------|
| `ros2 param list` | List all parameters |
| `ros2 param get <node> <param>` | Get parameter value |
| `ros2 param set <node> <param> <value>` | Set parameter value |
| `ros2 param dump <node>` | Save parameters to file |
| `ros2 param load <node> <file>` | Load parameters from file |

---

## Chapter 7 - ROS Actions

### 🎯 What is an Action?

**Action** is for long-running tasks with feedback.

**Service vs Action:**
| Feature | Service | Action |
|---------|---------|--------|
| Duration | Short | Long |
| Feedback | None | Yes |
| Cancel | No | Yes |
| Use Case | One-time request | Long task |

### 📋 Action Components

```
┌─────────────┐
│   Action    │
│   Client    │
├─────────────┤
│ 1. Goal     ← Send goal
│ 2. Feedback ← Get progress
│ 3. Result   ← Get final result
│ 4. Cancel   ← Cancel task
└─────────────┘
```

### 🔍 Managing Actions

```bash
# List all actions
ros2 action list

# Get action type
ros2 action type <action_name>

# Get action info
ros2 action info <action_name>
```

### 📤 Sending Goals

```bash
# Send action goal
ros2 action send_goal <action_name> <action_type> <goal_data>

# Example:
ros2 action send_goal /rotate_absolute turtlesim/action/RotateAbsolute "{theta: 1.57}"

# With feedback
ros2 action send_goal /rotate_absolute turtlesim/action/RotateAbsolute "{theta: 1.57}" --feedback
```

---

## Chapter 8 - ROS Workspace

### 📁 What is a Workspace?

A **Workspace** is where you store ROS2 packages.

### 🏗️ Workspace Structure

```
~/ros2_ws/                    ← Workspace root
├── src/                      ← Source code
│   ├── package_1/
│   ├── package_2/
│   └── package_3/
├── build/                    ← Build files
├── install/                  ← Installed packages
└── log/                      ← Log files
```

### 🔧 Creating a Workspace

```bash
# Create workspace directories
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws

# Build with colcon
colcon build

# Source the workspace
source install/setup.bash
```

### 📋 Workspace Management

```bash
# List packages
ros2 pkg list

# Find package location
ros2 pkg prefix <package_name>

# Install dependencies
rosdep install --from-paths src --ignore-src -r -y
```

---

## 📚 Summary

After completing this guide, you will understand:

- ✅ How to source ROS2 environment
- ✅ How packages and executables work
- ✅ How nodes communicate via topics
- ✅ How to use services for request/response
- ✅ How to configure nodes with parameters
- ✅ How to use actions for long tasks
- ✅ How to set up and manage workspaces

**Next Steps:**
1. Build Packages with Colcon
2. Create ROS Packages
3. Create Publisher/Subscriber in Python and C++
4. Launch Files
5. URDF and Xacro
6. RViz and Gazebo Simulation
7. SLAM and Navigation

---

**Author:** Fu (AI Assistant)  
**Last Updated:** 2026-04-24  
**Version:** 1.0 (Part 1 of 3)

---

**Keep going kun! 頑張ってください！** ✨🚀🐢

---

## Chapter 9 - Build ROS Packages with Colcon

### 🛠️ What is Colcon?

**Colcon** (Common Construction) is a tool for building ROS packages.

### 📦 Using Colcon

```bash
# Navigate to workspace
cd ~/ros2_ws

# Build all packages
colcon build

# Build specific package
colcon build --packages-select <package_name>

# Debug build
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug

# Parallel build (fastest)
colcon build --parallel-workers 4
```

### 📊 Build Options

| Option | Description |
|--------|-------------|
| `--packages-select` | Build specific package |
| `--packages-up-to` | Build with dependencies |
| `--parallel-workers` | Number of parallel builds |
| `--cmake-args` | CMake arguments |
| `--executor` | Executor type |

### 🧹 Cleaning Build

```bash
# Remove build directories
rm -rf build/ install/ log/

# Rebuild
colcon build
```

### ✅ Verify Build

```bash
# Check if package exists
ros2 pkg list | grep <package_name>

# Check executables
ros2 pkg executables <package_name>

# Source workspace
source install/setup.bash
```

---

## Chapter 10 - Create ROS Packages with Colcon

### 📦 Creating a Package

```bash
# Navigate to src folder
cd ~/ros2_ws/src

# Create Python package
ros2 pkg create --build-type ament_python <package_name>

# Example:
ros2 pkg create --build-type ament_python my_turtle_controller

# Create C++ package
ros2 pkg create --build-type ament_cmake <package_name>

# Example:
ros2 pkg create --build-type ament_cmake my_turtle_controller_cpp
```

### 📋 Package Structure

**Python Package:**
```
my_turtle_controller/
├── setup.py
├── setup.cfg
├── package.xml
└── my_turtle_controller/
    ├── __init__.py
    └── turtle_controller.py
```

**C++ Package:**
```
my_turtle_controller_cpp/
├── CMakeLists.txt
├── package.xml
├── src/
│   └── turtle_controller.cpp
└── include/
    └── my_turtle_controller_cpp/
        └── turtle_controller.hpp
```

### 📝 package.xml Example

```xml
<?xml version="1.0"?>
<package format="3">
  <name>my_turtle_controller</name>
  <version>0.0.0</version>
  <description>My turtle controller package</description>
  <maintainer email="kun@example.com">Kun</maintainer>
  <license>Apache-2.0</license>

  <depend>rclpy</depend>
  <depend>geometry_msgs</depend>
  <depend>turtlesim</depend>

  <export>
    <build_type>ament_python</build_type>
  </export>
</package>
```

### 🔧 setup.py Example

```python
from setuptools import setup

package_name = 'my_turtle_controller'

setup(
    name=package_name,
    version='0.0.0',
    packages=[package_name],
    install_requires=['setuptools'],
    zip_safe=True,
    maintainer='Kun',
    maintainer_email='kun@example.com',
    description='My turtle controller package',
    license='Apache-2.0',
    entry_points={
        'console_scripts': [
            'controller = my_turtle_controller.turtle_controller:main',
        ],
    },
)
```

### 🚀 Build and Run

```bash
# Go back to workspace
cd ~/ros2_ws

# Install dependencies
rosdep install --from-paths src --ignore-src -r -y

# Build
colcon build

# Source
source install/setup.bash

# Run
ros2 run my_turtle_controller controller
```

---

## Chapter 11 - Create Publisher and Subscriber in C++

### 📦 Creating C++ Package

```bash
cd ~/ros2_ws/src

# Create C++ package
ros2 pkg create --build-type ament_cmake cpp_talker_listener

# Add dependencies in package.xml:
# <depend>rclcpp</depend>
# <depend>std_msgs</depend>
```

### 📝 CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.8)
project(cpp_talker_listener)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)

# Publisher executable
add_executable(talker src/talker.cpp)
ament_target_dependencies(talker rclcpp std_msgs)

# Subscriber executable
add_executable(listener src/listener.cpp)
ament_target_dependencies(listener rclcpp std_msgs)

# Install
install(TARGETS
  talker
  listener
  DESTINATION lib/${PROJECT_NAME})

ament_package()
```

### 📤 Publisher Code (talker.cpp)

```cpp
#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"

class Talker : public rclcpp::Node
{
public:
    Talker()
    : Node("talker")
    {
        publisher_ = this->create_publisher<std_msgs::msg::String>("chatter", 10);
        timer_ = this->create_wall_timer(
            std::chrono::milliseconds(500),
            std::bind(&Talker::timer_callback, this));
    }

private:
    void timer_callback()
    {
        auto message = std_msgs::msg::String();
        message.data = "Hello ROS2! " + std::to_string(count_++);
        RCLCPP_INFO(this->get_logger(), "Publishing: '%s'", message.data.c_str());
        publisher_->publish(message);
    }

    rclcpp::TimerBase::SharedPtr timer_;
    rclcpp::Publisher<std_msgs::msg::String>::SharedPtr publisher_;
    size_t count_ = 0;
};

int main(int argc, char * argv[])
{
    rclcpp::init(argc, argv);
    rclcpp::spin(std::make_shared<Talker>());
    rclcpp::shutdown();
    return 0;
}
```

### 📥 Subscriber Code (listener.cpp)

```cpp
#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"

class Listener : public rclcpp::Node
{
public:
    Listener()
    : Node("listener")
    {
        subscription_ = this->create_subscription<std_msgs::msg::String>(
            "chatter", 10,
            std::bind(&Listener::topic_callback, this, std::placeholders::_1));
    }

private:
    void topic_callback(const std_msgs::msg::String::SharedPtr msg)
    {
        RCLCPP_INFO(this->get_logger(), "I heard: '%s'", msg->data.c_str());
    }

    rclcpp::Subscription<std_msgs::msg::String>::SharedPtr subscription_;
};

int main(int argc, char * argv[])
{
    rclcpp::init(argc, argv);
    rclcpp::spin(std::make_shared<Listener>());
    rclcpp::shutdown();
    return 0;
}
```

### 🚀 Build and Run

```bash
# In workspace
cd ~/ros2_ws

# Build
colcon build

# Source
source install/setup.bash

# Terminal 1: Publisher
ros2 run cpp_talker_listener talker

# Terminal 2: Subscriber
ros2 run cpp_talker_listener listener
```

### 📊 Output

**Publisher:**
```
[INFO] [talker]: Publishing: 'Hello ROS2! 0'
[INFO] [talker]: Publishing: 'Hello ROS2! 1'
[INFO] [talker]: Publishing: 'Hello ROS2! 2'
```

**Subscriber:**
```
[INFO] [listener]: I heard: 'Hello ROS2! 0'
[INFO] [listener]: I heard: 'Hello ROS2! 1'
[INFO] [listener]: I heard: 'Hello ROS2! 2'
```

---

## Chapter 12 - Create Publisher and Subscriber in Python

### 📦 Creating Python Package

```bash
cd ~/ros2_ws/src

# Create Python package
ros2 pkg create --build-type ament_python py_talker_listener
```

### 📝 setup.py

```python
from setuptools import setup

package_name = 'py_talker_listener'

setup(
    name=package_name,
    version='0.0.0',
    packages=[package_name],
    install_requires=['setuptools'],
    zip_safe=True,
    maintainer='Kun',
    maintainer_email='kun@example.com',
    description='Python talker and listener package',
    license='Apache-2.0',
    entry_points={
        'console_scripts': [
            'talker = py_talker_listener.talker:main',
            'listener = py_talker_listener.listener:main',
        ],
    },
)
```

### 📤 Publisher Code (talker.py)

```python
#!/usr/bin/env python3
import rclpy
from rclpy.node import Node
from std_msgs.msg import String

class Talker(Node):
    def __init__(self):
        super().__init__('talker')
        self.publisher_ = self.create_publisher(String, 'chatter', 10)
        self.timer = self.create_timer(0.5, self.timer_callback)
        self.count = 0
    
    def timer_callback(self):
        msg = String()
        msg.data = f'Hello ROS2! {self.count}'
        self.publisher_.publish(msg)
        self.get_logger().info(f'Publishing: "{msg.data}"')
        self.count += 1

def main():
    rclpy.init()
    talker = Talker()
    rclpy.spin(talker)
    talker.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

### 📥 Subscriber Code (listener.py)

```python
#!/usr/bin/env python3
import rclpy
from rclpy.node import Node
from std_msgs.msg import String

class Listener(Node):
    def __init__(self):
        super().__init__('listener')
        self.subscription = self.create_subscription(
            String,
            'chatter',
            self.listener_callback,
            10)
    
    def listener_callback(self, msg):
        self.get_logger().info(f'I heard: "{msg.data}"')

def main():
    rclpy.init()
    listener = Listener()
    rclpy.spin(listener)
    listener.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

### 🚀 Build and Run

```bash
cd ~/ros2_ws

# Build
colcon build

# Source
source install/setup.bash

# Terminal 1: Publisher
ros2 run py_talker_listener talker

# Terminal 2: Subscriber
ros2 run py_talker_listener listener
```

---

## Chapter 13 - Launch Files

### 🚀 What is a Launch File?

A **Launch File** allows you to start multiple nodes at once.

### 📝 Python Launch File Example

**File:** `launch/talker_listener_launch.py`

```python
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
    return LaunchDescription([
        Node(
            package='py_talker_listener',
            executable='talker',
            name='talker_node',
            output='screen'
        ),
        Node(
            package='py_talker_listener',
            executable='listener',
            name='listener_node',
            output='screen'
        )
    ])
```

### 📝 YAML Launch File Example

**File:** `launch/talker_listener_launch.yaml`

```yaml
launch:
  - node:
      pkg: py_talker_listener
      exec: talker
      name: talker_node
      output: screen
  
  - node:
      pkg: py_talker_listener
      exec: listener
      name: listener_node
      output: screen
```

### 🔧 Running Launch Files

```bash
# Python launch file
ros2 launch py_talker_listener talker_listener_launch.py

# YAML launch file
ros2 launch py_talker_listener talker_listener_launch.yaml
```

### 📦 Adding Launch Files to Package

**In setup.py:**

```python
from setuptools import setup
import os
from glob import glob

package_name = 'py_talker_listener'

setup(
    # ... other settings ...
    data_files=[
        ('share/ament_index/resource_index/packages',
            ['resource/' + package_name]),
        ('share/' + package_name, ['package.xml']),
        (os.path.join('share', package_name, 'launch'), glob('launch/*.py')),
    ],
    # ...
)
```

### 🎯 Launch File Benefits

- ✅ Run multiple nodes at once
- ✅ Easy parameter configuration
- ✅ Use conditionals
- ✅ Remap topics easily

---

**Author:** Fu (AI Assistant)  
**Last Updated:** 2026-04-24  
**Version:** 1.1 (Part 2 of 3)

---

**Halfway there kun! 頑張って！** ✨🚀🐢

---

## Chapter 14 - URDF Files

### 🤖 What is URDF?

**URDF (Unified Robot Description Format)** is an XML format for describing robot 3D models.

### 📝 URDF File Example

**File:** `urdf/my_robot.urdf`

```xml
<?xml version="1.0"?>
<robot name="my_robot">
  
  <!-- Base Link -->
  <link name="base_link">
    <visual>
      <geometry>
        <box size="0.5 0.3 0.2"/>
      </geometry>
      <material name="blue">
        <color rgba="0 0 1 1"/>
      </material>
    </visual>
    <collision>
      <geometry>
        <box size="0.5 0.3 0.2"/>
      </geometry>
    </collision>
    <inertial>
      <mass value="1.0"/>
      <inertia ixx="0.1" ixy="0.0" ixz="0.0"
               iyy="0.1" iyz="0.0"
               izz="0.1"/>
    </inertial>
  </link>
  
  <!-- Wheel Link -->
  <link name="wheel_link">
    <visual>
      <geometry>
        <cylinder radius="0.1" length="0.05"/>
      </geometry>
      <material name="black">
        <color rgba="0 0 0 1"/>
      </material>
    </visual>
  </link>
  
  <!-- Joint -->
  <joint name="wheel_joint" type="continuous">
    <parent link="base_link"/>
    <child link="wheel_link"/>
    <origin xyz="0.2 0.2 0" rpy="0 1.57 0"/>
    <axis xyz="0 0 1"/>
    <limit effort="10" velocity="1.0"/>
  </joint>
  
</robot>
```

### 🔍 Checking URDF

```bash
# Check URDF validity
check_urdf my_robot.urdf

# Visualize URDF structure
urdf_to_graphiz my_robot.urdf
```

### 📊 URDF Elements

| Element | Description |
|---------|-------------|
| `<link>` | Robot component |
| `<joint>` | Connection between links |
| `<visual>` | Visible shape |
| `<collision>` | Collision shape |
| `<inertial>` | Physical properties |

---

## Chapter 15 - URDF Xacro Files

### 📝 What is Xacro?

**Xacro (XML Macros)** makes writing URDF easier with macros and properties.

### 📄 Xacro Example

**File:** `urdf/my_robot.xacro`

```xml
<?xml version="1.0"?>
<robot name="my_robot" xmlns:xacro="http://www.ros.org/wiki/xacro">
  
  <!-- Properties -->
  <xacro:property name="wheel_radius" value="0.1"/>
  <xacro:property name="wheel_width" value="0.05"/>
  
  <!-- Macro for Wheel -->
  <xacro:macro name="wheel" params="prefix parent xyz">
    <link name="${prefix}_wheel">
      <visual>
        <geometry>
          <cylinder radius="${wheel_radius}" length="${wheel_width}"/>
        </geometry>
      </visual>
    </link>
    
    <joint name="${prefix}_wheel_joint" type="continuous">
      <parent link="${parent}"/>
      <child link="${prefix}_wheel"/>
      <origin xyz="${xyz}" rpy="0 1.57 0"/>
      <axis xyz="0 0 1"/>
    </joint>
  </xacro:macro>
  
  <!-- Base Link -->
  <link name="base_link">
    <visual>
      <geometry>
        <box size="0.5 0.3 0.2"/>
      </geometry>
    </visual>
  </link>
  
  <!-- Wheels -->
  <xacro:wheel prefix="front_left" parent="base_link" xyz="0.2 0.2 0"/>
  <xacro:wheel prefix="front_right" parent="base_link" xyz="0.2 -0.2 0"/>
  <xacro:wheel prefix="rear_left" parent="base_link" xyz="-0.2 0.2 0"/>
  <xacro:wheel prefix="rear_right" parent="base_link" xyz="-0.2 -0.2 0"/>
  
</robot>
```

### 🔧 Converting Xacro to URDF

```bash
# Convert xacro to urdf
xacro my_robot.xacro > my_robot.urdf

# Using ROS2 command
ros2 run xacro xacro my_robot.xacro
```

### 📦 Add to package.xml

```xml
<depend>xacro</depend>
```

---

## Chapter 16 - RViz Robot Simulation

### 🖥️ What is RViz?

**RViz (Robot Visualization)** is a 3D visualization tool for robot models and sensor data.

### 🚀 Starting RViz

```bash
# Start RViz2
rviz2
```

### 📝 RViz Config File

**File:** `rviz/my_robot.rviz`

```yaml
Panels:
  - Class: rviz_common/Displays
Visualization Manager:
  Displays:
    - Class: rviz_default_plugins/RobotModel
      Name: RobotModel
      Description Topic:
        Value: /robot_description
    - Class: rviz_default_plugins/TF
      Name: TF
  Global Options:
    Fixed Frame: base_link
```

### 🔧 Displaying URDF in RViz

```bash
# 1. Start robot state publisher
ros2 run robot_state_publisher robot_state_publisher --ros-args --param robot_description:=$(cat my_robot.urdf)

# 2. Start RViz
rviz2

# 3. In RViz:
#    - Add → RobotModel
#    - Description Topic: /robot_description
```

### 📊 RViz Display Types

| Display | Description |
|---------|-------------|
| RobotModel | Show URDF model |
| TF | Show transform frames |
| LaserScan | Show LiDAR data |
| PointCloud2 | Show 3D point cloud |
| Image | Show camera image |
| Path | Show robot path |
| Map | Show 2D map |

---
