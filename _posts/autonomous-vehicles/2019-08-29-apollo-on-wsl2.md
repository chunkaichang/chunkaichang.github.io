---
layout: single
title:  "Running Apollo on WSL2"
date:   2019-08-29 10:57:00 -0500
categories: 
  - autonomous-vehicles
  - tool
---

[Apollo][apollo] is an open-source autonomous driving platform developed by Baidu.
This post demonstrates how to run Apollo on top of WSL2, the second version of windows subsystem for Linux. 
Unlike WSL1 which emulates Linux system calls, WSL2 is a virtual machine and thus runs a Linux kernel.
Moreover, WSL2 now runs with Docker Destop in Windows. 
Docker is important as Apollo's dependencies are encapsulated within docker images.

Here are the steps:
1. Install and Configure WSL2
2. Install Docker Destop Tech Preview
3. Install and Run Apollo

> :warning: Steps in this post are subject to change in the future. The content is based on Windows 10 Education Insider Preview Build 18963 and Apollo 5.0 with git commit `320b77b5840a69836e56dcc1141a26fd2b631232`.

## Install and Configure WSL2
To use WSL2, we need a Windows version that supports Hyper-V and upgrade to Windows 10 Insider Preview builds.
Note that Windows Home does not support Hyper-V. I am using Windows Education. Look for discounts if you are in academia.
Once your machine supports Hyper-V, follow this [guide][ms-wsl2] from Microsoft to enable Insider Preview builds and 
install WSL2. This step might take several hours and several reboots are required to upgrade Windows.

Next, set WSL2 as the default WSL version by running `wsl --set-default-version 2` inside Powershell.
Download Ubuntu 18.04 from Microsoft Store. Check the WSL version by running `wsl -l -v` in Powershell.

To use GUI and web applications in WSL2, we need to tweak a little bit.
For GUI applications, we need to set the `DISPLAY` environment variable inside Linux to the WSL host IP address.
See this [issue][wsl2-gui] for instructions. 
You can also add the commands in this [script][set-display] to your `.bashrc` to perform the setting automatically.

Since Apollo provides a web GUI called Dreamview for visualization of the autonomous vehicle, 
we need to be able to use web applications in WSL2. 
According to this github [issue][wsl2-localhost], we need to bind the apps to `0.0.0.0` instead of the default localhost `127.0.0.1`.
See below for Apollo-specific instructions.

## Install Docker Destop Tech Preview
This tech preview allows the Ubuntu 18.04 inside WSL to interface with Docker Destop in Windows directly.
Follow this [guide][docker-wsl2] from Docker to install the special Docker Destop.
Make sure you turn on the tech preview before continuing.

## Install and Run Apollo
First, clone the lastest Apollo github repository to the Ubuntu 18.04.
Next, apply the following patch to `docker/scripts/dev_start.sh`:
```
334,335c334,335
<         --add-host in_dev_docker:127.0.0.1 \
<         --add-host ${LOCAL_HOST}:127.0.0.1 \
---
>         --add-host in_dev_docker:0.0.0.0 \
>         --add-host ${LOCAL_HOST}:0.0.0.0 \
```
This bind localhost to `0.0.0.0` instead of `127.0.0.1`.
Afterwards, follow this [guide][apollo-install] in the repository to install and run a demo.
What's cool here is that you can access `localhost:8888` in a web browser running in Windows. 


[apollo]: https://github.com/ApolloAuto/apollo 
[ms-wsl2]: https://docs.microsoft.com/en-us/windows/wsl/wsl2-install
[wsl2-gui]: https://github.com/microsoft/WSL/issues/4106
[set-display]: https://gist.github.com/ThYpHo0n/349f1f6473e207b866f65aca4728da3e
[wsl2-localhost]: https://github.com/microsoft/WSL/issues/4338
[docker-wsl2]: https://docs.docker.com/docker-for-windows/wsl-tech-preview/
[apollo-install]: https://github.com/ApolloAuto/apollo/tree/master/docs/demo_guide
