---
title: "Research"
permalink: /research/
---
I am doing research in the following areas:
- Error Injection and Acceleration 
- Functional Safety for Autonomous Vehicles

## Error Injection and Acceleration
Due to technology scaling and low-power design, computer systems are becoming sensitive to various hardware faults (e.g., particle strikes, voltage droop, wear-out, etc.). Although the majority of faults do not affect software, some lead to system/application crashes and some eventually corrupt the output of applications, rendering the system unreliable. I develop error injection tools to evaluate the efficacy of error detection mechanisms that protect systems from transient hardware faults. Moreover, I propose novel algorithms to accelerate error injection campaigns.

<!--
*Error Modeling* is about how to model various hardware faults that propagate through abstraction layers. An analogy is modeling how disease-producing agents (e.g., viruses and bacteria) affect a human body. Such biological propagation process is very complicated, so is its counterpart in computer systems. Prior work assumes that simple error models (e.g., single-bit flips) represent realistic hardware errors. My work generates realistic hardware errors using just in time gate-level fault injection to improve modeling fidelity.

*Error Injection* refers to the experiments of injecting hardware errors into the target system and observing their impact on applications. Depending on the fault type, the hardware state, and the application state, the impact can be quite different. For instance, the fault might slowdown, crash, or even malfunction the application. Through error injection experiments, we can identify the vulnerable regions of the application and evaluate the efficacy of error-mitigation techniques.  
-->
I am currently developing [Hamartia](http://lph.ece.utexas.edu/users/hamartia/){:target="_blank"}, an error injection and detection tool. 

## Functional Safety for Autonomous Vehicles
As human errors account for >90% of car accidents in the US, Autonomous Driving is an emerging technology to improve road user safety. 
Since autonomous vehicles heaviliy depend on electronic devices to fulfill their missions, standards such as ISO 26262 state that it is cruical to address the effects of hardware faults on functional safety of autonomous vehicles. Error injection is thus neccessary to ensure that autonomous vehicles meet safety requirements.

My focus is using error injection to understand the impact of random hardware faults on software that runs in the vehicle. 
The end goal is to design safety mechanisms with low performance overhead.


