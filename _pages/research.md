---
title: "Research"
permalink: /research/
---

## Error Modeling & Injection
Due to technology scaling, computer systems are becoming sensitive to hardware faults from a variety of sources (e.g., particle strikes, voltage droop, wear-out, etc.). Some hardware faults eventually become errors that affect the output of applications, rendering the system unreliable.

*Error Modeling* is about how to model various hardware faults that propagate through abstraction layers. As a analogy, it is like modeling how disease-producing agents (e.g., viruses and bacteria) affect a human body. Such biological propagation process is very complicated, so is its counterpart in computer systems. 

*Error Injection* refers to the experiments of injecting hardware errors into the target system and observing their impact on applications. Depending on the fault type, the hardware state, and the application state, the impact can be quite different. For instance, the fault might slowdown, crash, or even malfunction the application. Through error injection experiments, we can identify the vulnerable regions of the application and evaluate the efficacy of error-mitigation techniques.  

I am currently developing [Hamartia](http://lph.ece.utexas.edu/users/hamartia/){:target="_blank"}, an error injection and detection tool. 

