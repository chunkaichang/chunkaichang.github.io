---
layout: single
title:  "Elements of Autonomous Vehicles - Part 1: Sense"
date:   2019-07-13 22:36:00 -0500
categories: autonomous-vehicles
---

A generic autonomous vehicle consists of three main stages: Sense, Plan, and Action.
In this post, I will focus on the elements of the Sense stage first along with why and how they fail.

## Perception Sensors
Multiple sensors are used in tandem to accurately sense the environment and geographic location.
To complement the weakness of each sensor, *Sensor Fusion* is widely used to aggregate input data collected by multiple sensors as a form of redundancy.

- Environment perception sensors (a.k.a. on-board sensors)
    - Camera: for object classification
    - LIDAR: for identifying structured and unstructured objects.
    - RADAR: for sensing moving objects.
    - Ultrasonic: for measuring the shortest distance to objects nearby 
    - Microphone: e.g., for sensing railway intersection
- A-priori perception sensors (a.k.a off-board sensors)
    - HD Maps
    - Global Navigation Satellite System (GNSS)
    - [Vehicle-to-everything (V2X)][v2x]


## Data Flows
The following figures are from a recent publication: Saftey First for Automated Driving [(pdf)][intel-safety].
Determining location:
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/av/FS_1.PNG){: .align-center height="300px" width="600px"}
Egomotion here describes the state of the vehicle (including velocity, acceleration, etc.).

Determining world model:
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/av/FS_2.PNG){: .align-center height="300px" width="600px"}



## Root Cause of Failures
Environment sensors are sensitive to the operating conditions. For instance, cameras are sensitive to the lighting and weather conditions.
Noises from the environments may transform the input into adversarial examples and thus fail elements in subsequent stages.

An HD map can fail when the map information deviates from reality. 
Potential root causes are erroneous data collection process and real-world changes.
Real-world changes can be further classfied into three categories: 
(1) planned changes (e.g., planned road construction),
(2) unintented changes (e.g., damaged traffic signs),
and (3) malicious changes.
Actions must be taken to make sure the map information is up to date and intact.

A GNSS can fail due to loss of connection to satellites (e.g, the vehicle enters a tunnel).
In such scenarios, location information must be collected from other sources (e.g., wireless APs in the tunnel).

--------------------------------------
[intel-safety]: https://newsroom.intel.com/wp-content/uploads/sites/11/2019/07/Intel-Safety-First-for-Automated-Driving.pdf
[drive-fi]: https://saurabhjha1.github.io/resources/DSN2019.pdf
[v2x]: https://en.wikipedia.org/wiki/Vehicle-to-everything
