---
layout: post
title: Simulation Project – Rationale and Goals
---

I am spending the five weeks of March Term and Spring Break working on this, with plans to continue this project in the summer.

# Overall Goals

1. Develop a self-contained framework necessary to simulate and test the efficacy of algorithms for a multi-agent system (e.g. drones, ground robots) , with an easy-to-use and documented API with cross-language support.

2. Develop, implement, and test algorithms with a specific application in mind. Identify other possible applications of such a system.

# Background and Rationale

The idea for this project began s the research I conducted at the University of California, Santa Barbara in the summer of 2019. I focused on simulating a system where a set of distributed drones have to maximize their combine coverage in a weighted environment, but in such a way where each drone makes its own decisions. There is no centralized server or node providing instructions to all other drones. The drones also have limited prior knowledge about their environment: they make their decisions based solely on the locations of the drones closest to them and the "weight," or importance, of covering any given location. A way to visualize such an environment is that there is a grid of squares, each assigned a weight value, and each drone has a circle around it that covers a certain number of squares, depending on its sensing radius. The sum of the values that it covers that is not already covered by another drone is its "utility."

I began by testing the baseline approach to any computer science approach: a greedy algorithm, in which each drone would move to any location within its immediate vicinity that provided a higher total "utility." The issue with this was that like most greedy algorithms, it only found a locally optimal solution. After this, I found a more complex algorithm in the literature known as a "log-linear learning algorithm", which encourages drones to make moves occasionally at random to increase the probability of finding a global optimum. My contribution to this algorithm was automatically generating a constant that determined the amount of noise to add to the system, and creating a decay so that the system would gradually converge over time.

Although I was able to "simulate" my algorithms through MATLAB code in some capacity, there were a few limitations I faced. I wasn't able to create a truly distributed or multi-threaded environment, because all decisions were still happening in one loop with access to the same information. As a result, the decisions were actually sequential, and all drones had access to perfect information and communication with all other drones. Additionally, the system did not closely model a true drone system, where there would be latency in communication and the data would have to be packaged in a way that each drone could quickly update its understanding of the environment. Unlike the system that I simulated, in reality no drones could have completely up-to-date information about its environment; rather, it would have to make decisions based on potentially stale information and be able to adapt if something changed. Other constraints I was unable to incorporate in the simulation include battery life, where drones would have to stop at charging ports periodically. Additionally, the simulation did not simulate movement very accurately: it was only in a discrete grid, and only in two dimensions. In order to truly verify the efficacy of the algorithm I implemented and enhanced, I would need to specify how to handle some of these conditions and test the algorithm in a more accurate simulation environment.

I imagine that I would not be the only person without access to an unlimited number of drones for testing on demand who would like to be able to accurately test their algorithms before publishing papers or moving forward with research. Some such tools already exist, most notably Georgia Tech's Robotarium, which is a remote access robotics lab for researchers to test their algorithms for swarm robotics on real robots. While this is a great advance in increasing the access of multi-robot systems testing to anyone around the world, the Robotarium has some limitations. For example, it only allows for MATLAB and Python APIs, which can be difficult to interface with when using code in Java/C/C++ or other languages. As with any physical testing system, it has a limited number of robots, and the robots also don't have 6 degrees of freedom: they use a differential drive control system, and also only move in two dimensions.

# Specification

With this in mind, I have decided to develop a simulation system to address these issues. This system will have the following capabilities:
- Backend for the framework is written in Kotlin
- Cross-platform API using gRPC that allows the user to use any language to:
- Define a discrete static or dynamic weighted map, but which allows interpolation of weight values in intermediate locations
- Spin up any number of agents, each of which are Docker instances. The agents can
    - move based on an algorithm (see below)
    - have the option to "act" on their environment, which could affect the weighted map
    - have the option to "sense" information from the environment, which could affect the weighted map and its movement algorithm
- Define a movement algorithm for the drones that
    - May be in a discrete (less realistic) or continuous (more realistic) way
    - Allows the user to specify whether it is a 1D, 2D, or 3D environment
- Specify the type of communication
    - Fully connected: each drone can communicate with every other drone
    - Hub/spoke (also known as master/slave): there is one central drone that every other one is connected to (and ways to transfer the master position)
    - Each agent is connected to one other agent
    - Each agent can communicate with the closest other agent
    - Each agent can communicate with any agent in a given distance
- Specify what information is passed on in the communications between agents, and what information each agent has access to
    - For instance, in the UCSB research, each drone could only access a segment of the weighted map at a time, which it had to "sense" -- there was no communication between the drones
    - Drones may pass on their current understanding of the environment, or only what they sense, and it is up to each drone to update its picture of reality
- Be able to add any other type of limitation
    - Battery Life = time before visiting a "home base"
    - Minimum distance between each other
    - Define obstacles so the drones go around these areas

The minimum viable product will allow me to simulate the same environment and algorithm as the one I worked on at UCSB. The requirements for this are:
    - 2-dimensional discrete movement
    - Static weighted map of the environment
    - Drones can communicate a universal shared map
