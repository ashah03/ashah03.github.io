---
layout: post
title: Running Maker Faire Booth for Robolink in San Diego
---
Last weekend, Donovan and I flew down to San Diego to help run Robolink's Maker Faire booth there. For the last month, we worked to get our Machine Learning demo working. Our goal was to get the Zumi to be able to distinguish between a coke can and an apple and react accordingly by driving either forwards or backwards. An added constraint was that the model would have to be small enough to run quickly on a Raspberry Pi Zero, a computer that has less than 0.5 GB of RAM and a very small CPU.

# Creating the Demo

## Data

We began by collecting the data with a Jupyter Notebook that took a picture every 0.5 seconds and displayed the picture on the laptop screen. We took approximately 1000 photos each of the apple and the coke can. We decided to use Google Colaboratory to train the model, so we uploaded all of the photos to Google Drive. We used Keras for the data augmentation and training. The Keras ImageDataGenerator took care of the data augmentation (we chose to use rescaling, zooming, shearing, and flipping) and prepared the data for training.

## Training

For the actual model, we used the Keras Sequential API. The architecture of the Neural Network was fairly straightforward: we used a series of convolutional and max-pooling layers, followed by a Dense layer.

## Results

We achieved about 96% training accuracy and 94% validation accuracy. In practice, it worked almost every time (while using the same apple), but it took about 2-3 seconds to process each frame on the Raspberry Pi's tiny CPU.
[The code can be found here.](https://colab.research.google.com/drive/1iI09ihylVAWu933yQ1ehxFw7a0NQmryA)

# Running the Maker Faire Booth

When we arrived at the Maker Faire on Saturday, we realized that the demo wasn't working. (Lesson: Nothing never works when you need it to.) The first issue we had to resolve was not being able to connect the Raspberry Pi to the internet. We were then left to debug the code and figure out what the issue was. After we finally dug through the code (and found the issue), we had the code up and running by about an hour after the faire had opened.

Our demo was one of three or four that various Robolink employees had developed, and many of them were very impressive. There were also multiple pamphlets advertising their robotics products and classes. The first issue that I saw in the booth was that they had all of the engineers there, but no one who was experienced in marketing or sales.

The issues just began there. The booth was in a prime location: the first thing everyone saw after entering the robotics building there. And yet the entrance to the booth was on the opposite side of the booth, so no one would enter. When I convinced them to move one of the tables such that the entrance would be at the front, the traffic into the booth must have tripled.

Another thing I noticed was that the engineers would showcase their demos, but they would stop there, and not tell the intrigued customers how to proceed. I took that opportunity to advertise Robolink's classes, products, and summer programs, not hesitating to give out the pamphlets to anyone who seemed remotely interested, so that the company's investment in the maker faire booth would actually be worth it for them financially.

This experience taught me that there is so much more to selling a product or service than having cool demos and writing great code. It is necessary to communicate and sell your product or service in a way that catches someone's attention, and then direct them to ways that they can keep their interest piqued. 




