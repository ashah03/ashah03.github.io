---
layout: post
title: Simulation Project – April Update
---
In the last five weeks since I began this project in earnest (the vast majority of which I spent cooped up at home due to quarantine), I have accomplished quite a lot! In this post, I will discuss what I have accomplished so far, a change in project scope, takeaways from the various technologies I have worked with, and the next milestones for the project. The code for this project can be found at my GitHub [here](https://github.com/ashah03/kotlin-distributed-framework).

# Accomplishments during March Term and Spring Break

- Created a Java/Kotlin API 
- Wrote a DSL (domain-specific language) in Kotlin
    - Allows for a user-friendly experience for declaring parameters for the nodes, and specifying the algorithm. Currently only has support for discrete and iterative algorithms (can only visit integer coordinate locations, and occurs on a timed cycle).
- Create the ability to spin up any number of agents
- Create objects that the user can use for storing locations, weights, etc.
- Completed the communication capability with gRPC, none of which is exposed to the user. 
    - This takes care of the coordination of location and weight information between all nodes. Although it is a server-client model in the backend, it simulates a "broadcast" message, where the server forwards any messages it receives to all other drones. This allows for analytics to occur, and restrict the messages to specific nodes. The communication is also extensible to other communication types.
- Began implementing the algorithm from my UCSB research, which informed the design of the DSL

## Change in Project scope 

- I have realized that it is not plausible to create cross-language support for a platform such as this without developing in each of those languages, especially because the users need to be able to pass in logic. Consequently, the current plan is to have the platform compatible with any JVM language, with examples provided in Kotlin and Java.

# Takeaways from Technologies Used

## Kotlin 

I have come to realize how amazing of a programming language Kotlin is. It combines the functional and object-oriented programming paradigms to create an environment with the best of both worlds. Here are some of my favorite capabilities of Kotlin that I used in this project

- Lambdas
    - This is the core of Kotlin's support for the functional-programming paradigm. It allows functionality to be treated as "first-class citizens" of the language, just like any variable containing data.

- Functions with Lambda Arguments
    - A function in Kotlin that has only one argument, a lambda, can omit the parenthesis. This makes the function appear as a keyword, which allowed me to create the DSL. 
    - For instance, I have a function defined with the following declaration
    `fun config(function: APIBuilder.() -> Unit)`
    - Moreover, the component `APIBuilder.()` provides the context of the object `APIBuilder`, so that any code within the `config{}` blog is written as if from within the `APIBuilder` object. In the implementation of the config method, the function can be called as an instance method of the object, as in `apiBuilder.function()`, even though it is not contained within the class code.
 - Extension Functions
    - The capability described above, where functions can be added after the fact (even to built-in objects) and be used as instance methods, is known as an extension function. Another use for this is to make the code more clear and elegant when certain actions are repeated often. 
   - For instance, the code `Coordinate(Random.nextInt(0, 10), Random.nextInt(0, 10), Random.nextInt(0, 10))` can be reduced to 
   `Coordinate(10.random, 10.random, 10.random)`
   
   - In this code, `Int.random` is declared as an extension function which calls `Random.nextInt(0, 10)`, which is possible though the Int class is private and located in the Kotlin standard library.


## gRPC

Through this project, I have learned that gRPC is a great tool for communication at any scale, whether between a single thread, many threads of the same program, separate threads, or even separate machines (in my case, Docker containers). Here are some of my favorite aspects of gRPC:

- Protobuf (Protocol Buffers)
    - Although the name may make it seem as if Protobuf is an old and hard-to-use technology, this is far from the case. Protobuf forms the core of gRPC, because it allows developers to specify message types in a simple, cross-platform manner. 

    - Once message types are created, you can specify various RPCs or Remote Procedure Calls. These include asynchronous requests or streams.

- Auto-Generated Code

    - One the .proto file is created, gRPC will automatically generate code in any language that creates this message objects and RPCs methods. Whether from Python, Java, C, or anything else, the user can call any of the RPCs without ever leaving the language environment, and the gRPC backend will send the objects to the receiver and execute the code specified. 

gRPC essentially replaces HTTP and REST APIs, because it allows much more powerful messages to be sent between the server and client. In my project, I used gRPC to communicate between the nodes and the communication server, taking advantage of its duplex streaming capability, which allows information to be streamed simultaneously in both directions, to simulate broadcast messages.


## Docker

Docker is an amazing tool for deploying applications without worrying about the environment. Although I didn't directly take advantage of this capability, I was pleasantly surprised by how easy it is to define the requirements of a container (in my case, a drone or any other node in a multi-agents system), and spin up an arbitrarily large number of them! 

# Next Steps

- Implement Greedy and Log-Linear-Learning algorithm from UCSB Research
    - This will help inform the next steps for continuing work on the DSL. 
- Create a more general DSL object that doesn't require iterative movement or discrete motion
- Documentation!!
    - Create a README on the GitHub repository on how to use the Kotlin DSL (preferred) and the Java/Kotlin/JVM API
    - A few examples, including the two above, and how they work
- Dashboard / Analytics / Visualization
    - Create a way for the users to see analytics on their system, including how long the algorithm takes to process each movement decision, how long the system to complete the prescribed task, etc.
    - Create a visualization (at least 1D and 2D) for the movement of the nodes, so that the user can see visually how well the algorithm is performing
    - Combine all of this into a user-friendly dashboard, possibly a web-app or desktop app of some sort?
- Communication Restrictions
    - Allow the users to specify restrictions to communication as described in the original blog post, e.g. each drone can only communicate with n others, or a hub-spoke model. 
- Other Limitations / Restrictions
    - Include the ability to add restrictions such as battery life (see [original blog post](/Simulation-Project-1))
      
