# **Self-Driving Car**
# **Capstone Project: Programming A Self Driving Car**

## MK

#
Overview

A system integration project to drive Carla, the Udacity self-driving car, around a test track. For this project, Robot Operating System (ROS) was utilized to implement core functionality of the autonomous vehicle system: traffic light detection, control, and waypoint following. 

As this was an individual submission, code testing was performed only on the simulator.

#
Carla Architecture

Carla is a Lincoln MKZ vehicle, modified into a self-driving car. It's self-driving system is broken down into four major sub-systems: Sensors, Perception, Planning, and Control

The following is a system architecture diagram showing the ROS nodes and topics used in the project. You can refer to the diagram throughout the project as needed. The ROS nodes and topics shown in the diagram are described briefly in the Code Structure section below, and more detail is provided for each node in later classroom concepts of this lesson.

#
The Project

The steps for this project:

* Waypoint Updater Node (Partial): complete a partial waypoint updater which subscribes to t/base_waypoints and /current_pose and publishes to /final_waypoints.
DBW Node: Once your waypoint updater is publishing /final_waypoints, the waypoint_follower node will start publishing messages to the/twist_cmd topic. At this point, you have everything needed to build the dbw_node. After completing this step, the car should drive in the simulator, ignoring the traffic lights.
Traffic Light Detection: This can be split into 2 parts:
Detection: Detect the traffic light and its color from the /image_color. The topic /vehicle/traffic_lights contains the exact location and status of all traffic lights in simulator, so you can test your output.
Waypoint publishing: Once you have correctly identified the traffic light and determined its position, you can convert it to a waypoint index and publish it.
Waypoint Updater (Full): Use /traffic_waypoint to change the waypoint target velocities before publishing to /final_waypoints. Your car should now stop at red traffic lights and move when they are green.

* Build a PID controller and tune the PID hyperparameters
* Test the tuned PID controller on the vehicle/race-track simulator
* Vehicle must successfully drive a lap around the track
* There is no minimum vehicle speed criteria. However, maximize vehicle speed by tuning the PID parameters

