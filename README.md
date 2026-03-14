# ROS2 Nav2 Navigation Assignment

## Overview

This repository contains the implementation of autonomous navigation for the Testbed robot using the **ROS2 Navigation Stack (Nav2)**. The robot is simulated in Gazebo and visualized in RViz2. The system loads a pre-built map, performs localization using AMCL, and navigates to user-defined goals using the Nav2 planner and controller.

The project demonstrates the full navigation pipeline in ROS2 including:

* Robot simulation
* Map loading
* Localization
* Path planning
* Autonomous navigation

---

# Tools and Software Used

### 1. ROS2 Humble

Robot middleware used to manage nodes, topics, services, and communication between different components of the robot system.

### 2. Gazebo Simulator

Used to simulate the robot and environment.

Purpose:

* Run the robot inside a virtual world
* Test navigation algorithms without real hardware

### 3. RViz2

Visualization tool used to visualize:

* Robot model
* Map
* Laser scans
* TF frames
* Navigation goals
* Planned paths

### 4. Nav2 (Navigation2 Stack)

Nav2 is the ROS2 navigation framework responsible for autonomous robot movement.

Components used:

* map_server
* AMCL (Adaptive Monte Carlo Localization)
* Planner Server
* Controller Server
* Behavior Tree Navigator

---

# Algorithms Used

## 1. AMCL (Adaptive Monte Carlo Localization)

AMCL is a probabilistic localization algorithm that estimates the robot’s position on the map using a particle filter.

Input:

* Laser scans
* Odometry
* Map

Output:

* Robot pose estimate in the map frame

---

## 2. Global Path Planning

Nav2 computes a global path from the robot’s current location to the goal position using the map.

Planner used:

* NavFn / A* based planner

Purpose:

* Find the shortest obstacle-free path.

---

## 3. Local Path Planning / Controller

The controller follows the global path and generates velocity commands.

Responsibilities:

* Avoid obstacles
* Smooth path following
* Adjust motion dynamically

---

# Repository Structure

```
ros_nav2_assignment
│
├── testbed_description
│   ├── launch
│   ├── meshes
│   ├── rviz
│   └── urdf
│
├── testbed_gazebo
│   ├── launch
│   ├── models
│   └── worlds
│
├── testbed_bringup
│   ├── launch
│   └── maps
│
├── testbed_navigation
│   ├── launch
│   ├── config
│   ├── CMakeLists.txt
│   └── package.xml
│
├── README.md
├── help.md
└── .gitignore
```

---

# Steps to Run the Simulation

## 1. Build the Workspace

Navigate to the workspace:

```
cd ~/assignment_ws
```

Build the workspace:

```
colcon build
```

Source the workspace:

```
source install/setup.bash
```

---

# Step 2 — Launch the Simulation

Run the simulation which launches Gazebo, robot description, and RViz:

```
ros2 launch testbed_bringup bringup.launch.py
```

This will start:

* Gazebo simulator
* Robot model
* RViz visualization

⚠️ **Important Note**

Sometimes Gazebo or RViz may not load properly on the first attempt.
If the simulation does not start correctly, simply stop the launch and run the command again.

---

# Step 3 — Load the Map

Launch the map server:

```
ros2 launch testbed_navigation map_loader.launch.py
```

This loads the map using the Nav2 map server.

The map will be published on:

```
/map
```

---

# Step 4 — Start Localization

Run the localization node:

```
ros2 launch testbed_navigation localization.launch.py
```

This launches AMCL for robot localization.

---

# Using RViz2 for Navigation

Once RViz2 opens, follow these steps:

## 1. Set Fixed Frame

In RViz:

```
Global Options → Fixed Frame → map
```

---

## 2. Add the Map Display

If the map is not visible:

1. Click **Add**
2. Select **Map**
3. Set topic to:

```
/map
```

The occupancy grid map should appear with walls and free space.

---

## 3. Check Robot Model

Ensure the robot model is visible in RViz.

Displays to check:

* RobotModel
* TF
* LaserScan

---

## 4. Set the Initial Pose

Before navigation, set the robot’s initial pose.

Steps:

1. Click **2D Pose Estimate**
2. Click on the robot location on the map
3. Drag the arrow to set the robot orientation

This initializes AMCL.

---

## 5. Send Navigation Goal

To move the robot:

1. Click **2D Nav Goal**
2. Click on the desired destination on the map
3. Drag the arrow to set the orientation

The arrow indicates the **final heading direction** of the robot.

After setting the goal:

* Nav2 planner calculates a path
* Controller moves the robot
* Robot navigates to the target position

---

# Important RViz Tools

### 2D Pose Estimate

Used to initialize robot pose for localization.

### 2D Nav Goal

Used to send navigation goals.

### Arrow Direction

Defines the final orientation of the robot at the goal.

---

# Notes and Observations

1. The robot may take some time to reach the goal depending on path complexity.
2. The robot may rotate near the goal to achieve the correct orientation.
3. Occasionally the simulation may fail to start properly on the first attempt.
4. If Gazebo or RViz does not load correctly, simply terminate the launch and run it again.

---

# Expected Output

After following all steps:

* The robot appears in Gazebo.
* The map appears in RViz.
* Localization starts using AMCL.
* The robot successfully navigates to user-defined goals.

---

# Conclusion

This assignment demonstrates the implementation of a full navigation pipeline using ROS2 Nav2. The robot successfully performs localization, path planning, and autonomous navigation within a simulated environment.

The integration of Gazebo simulation and RViz visualization enables testing and validation of navigation algorithms without requiring physical hardware.
