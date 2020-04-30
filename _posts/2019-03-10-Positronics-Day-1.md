---
layout: post
title: Positronics – Day 1
---

Today was my first day at Positronics, a robotics software company, and I will be working here for the next three and a half weeks for our school's inaugural March Term.

# The System

My goal is to optimize a robotics manufacturing process that Positronics has developed. The system assembles a heating plate for a vacuum chamber that is used for semiconductor manufacturing in a clean room environment. The system consists of a 6-axis [Universal Robot Arm](https://www.universal-robots.com/), which has two attachments: [Robotiq](https://robotiq.com/) gripper, used for handling bolts, and a screwdriver (allen key).

In order to assemble the vacuum plate, these are the steps the system must take:

1. Attach the gripper attachment to the robot arm
2. Pick up the bolts from a bolt holder and place them in each of the 40 holes on the plate
3. Screw all bolts to the following torque settings:
- 5 lb-in
- 20 lb-in
- 40 lb-in
- 60 lb-in
4. Verify the 60 lb-in of torque twice

# Optimization

The system is already in deployment, but is in need of optimization. There are two main factors that need to be optimized:

- Reducing cycle time
- Ensuring that a clean-room environment is maintained

For the former, actions that can be taken include increasing the max speed and/or acceleration, as well as using blend radii for intermediate waypoints for the arm. This involves coming close to the waypoint, but not exactly reaching it, which allows the robot to continue moving throughout the process. Additionally, there are many waypoints that may not be necessary, and are used to ensure that the robot is in the right spot; this could be resolved by reducing the number of waypoints and instead defining zones that the robot cannot pass through.

The next place for optimization is ensuring that the screwdriver consistently lands in the bolt; this would impact both cycle-time and clean-room. If the tool is not precisely aligned, and is spun at high speed, it can create metal shavings, contaminating the environment. Furthermore, the more times the system has to attempt to place the tool, the longer the cycle-time. 

What I worked on today was calibrating the system so it knows exactly where the end-effector, or the end of the allen-key, is relative to the wrist of the robotic arm. This allows the robotic arm to rotate and move around while maintaining the location of the end of the tool. (Currently, this is what is being done to help position the tool inside the bolt hole: angling the tool and then straightening again.) 

I will begin by experimenting with various motions the tool can be rotated to best ensure that it lands inside the hole. Alternatively, the hole could move slightly in the x and y directions, until the arm feels an amount of force greater than a threshold (indicating that it is hitting something) and the (x,y) coordinate is below the surface of the plate (indicating that it is indeed inside the hole).






