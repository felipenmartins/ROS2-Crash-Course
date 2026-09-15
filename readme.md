# ROS 2 Crash Course

Welcome to the ROS 2 Crash Course!

This is a hands-on ROS 2 crash course covering ROS fundamentals and robot programming in Python through practical activities with TurtleSim (and optionally with the iRobot Create3).

This course's page is available at: [https://felipenmartins.github.io/ROS2-Crash-Course/](https://felipenmartins.github.io/ROS2-Crash-Course/).

As the name suggests, this is a _crash course_. It was designed to give you a quick start and help you gain foundational knowledge to allow you to independently learn new ROS concepts as you need them in the future.

> **_Note:_** This is a course about ROS 2, not about the fundamentals of robotics. If you are interested in learning more about robotics concepts, check out my [Robotics Simulation Labs](https://felipenmartins.github.io/Robotics-Simulation-Labs/) and accompanying [Jupyter Notebooks for learning Mobile Robot Control](https://github.com/felipenmartins/Mobile-Robot-Control).

## Structure

ROS 2 Crash Course is organized in 2 parts: Part 1 covers core concepts of ROS 2, whereas Part 2 is dedicated to the application of such concepts on the Create3 robot. Part 2 is optional.

The course structure is:

### Part 1 - ROS

This part covers fundamental ROS 2 concepts. It can be followed even if you don't have access to a physical robot.

- [**Chapter 1 - ROS 2 Introduction: Foundations and Core Concepts**](/Part_1-ROS/Chapter-1/readme.md)
  - ROS overview and history
  - Why use ROS?
  - Main concepts (packages, workspaces, nodes, topics etc.)
    - _Activity: Creating your own workspace and running your first nodes_

- [**Chapter 2 - ROS 2 Building Blocks: Nodes, Topics, Services, and Actions**](/Part_1-ROS/Chapter-2/readme.md)
  - TurtleSim
  - Nodes
    - _Activity: Running and inspecting nodes_
  - Topics
    - _Activity: Working with topics_
    - _Activity: Writting Python code for topics_
  - Actions and Services
    - _Activity: Getting familiar with actions and services_

- [**Chapter 3 - ROS 2 Tools: Launch Files, Bags, Visualization, and Simulation**](/Part_1-ROS/Chapter-3/readme.md)
  - Launch files
    - _Activity: Creating a Launch File_
  - ROS Bags
    - _Activity: Recording and playing back data with ROS Bags_
  - Transforms (TFs) and coordinate frames
    - _Activity: Understanding TFs_
  - RViz - ROS Visualization tool
    - _Activity: Visualizing TFs with RViz_
  - Gazebo
  - Packages
    - _Activity: Adding Create3 packages to run a simulation_

### Part 2 - Create3

This part contains a quick introduction to the [iRobot Create3](https://iroboteducation.github.io/create3_docs/) educational robot using iRobot's Python Web Playground, followed by activities to practice ROS 2 concepts with the robot.

- [**Chapter 4 - Introduction to the iRobot Create3**](/Part_2-Create3/Chapter-4/readme.md)
  - Robot Overview
  - Powering the robot ON
  - Powering the robot OFF
  - _Activity: Programming the Create3 with Python Web Playground_

- [**Chapter 5 - Create3 Topics**](/Part_2-Create3/Chapter-5/readme.md)
  - _Activity: Preparing the Create3 for ROS_
  - _Activity: Investigating the Create3 topics_
  - _Activity: Programming the Create3 with ROS 2_

- [**Chapter 6 - Create3 Actions**](/Part_2-Create3/Chapter-6/readme.md)
  - List of all available actions
  - _Activity: Call actions to control the robot_
  - _Activity: Call actions from ROS nodes_

## Intended Learning Outcomes

By the end of this course, you should be able to:

- Understand fundamental concepts of ROS 2.
- Run ROS 2 commands and nodes from the terminal.
- Write Python code to develop custom ROS 2 packages that include custom nodes.
- Interact with and program ROS-enabled robots.

## Requirements

### Linux command line

This course assumes that you are familiar with the Linux Terminal commands. You don't need to be an advanced Linux user, but you should know how to:

- access the command line
- investigate and navigate the folder structure (list and access files and folders)
- perform basic file manipulation (create, delete, copy and move files)
- run a command as a super user

If you are new to Linux, I recommend the tutorial [The Linux command line for beginners](https://ubuntu.com/desktop/docs/en/latest/tutorial/the-linux-command-line-for-beginners/), which covers all above topics.

### Software

The software requirements for this course are:

- [Ubuntu 24.04](https://releases.ubuntu.com/24.04/)
- [ROS 2 Jazzy Jalisco](https://docs.ros.org/en/jazzy/index.html)
- [iRobot Create3 Platform interfaces](https://github.com/iRobotEducation/irobot_create_msgs) package - required to work with the Create3 robot.

### Virtual Machine

Preferably, you should have a machine running Ubuntu as main operating system or in dual boot. However, for this course a virtual machine also works.

> **A guide to create a Virtual Machine with Ubuntu and ROS 2 is [available here](/Part_1-ROS/ros2_vm_guide.md).**

### Hardware

The demonstration platform used in this course is the [iRobot Create3](https://iroboteducation.github.io/create3_docs/) educational robot. You do not need any robot hardware to complete Part 1, but a working Create3 robot is required to complete the activities in Part 2.

## Acknowledgment

An early version of this course was prepared by Marwan Refaat during a summer internship at Hanze University of Applied Sciences under my supervision. This repository was forked from his original [ROSWorkshop](https://github.com/Marwan-Refaat/ROSWorkshop) and was built on top of his work.

Thank you, Marwan!

## License

Just like Marwan's work, this project is licensed under the terms of the [MIT license](/LICENSE.md).
