---
title:  "Comma Docs"
mathjax: true
layout: post
categories: machine-learning
---

OpenPilot from comma.ai heavily relies on multiple daemons that interact via message passing with each other. The core services can be found inside `selfdrive/` and `tools/`. A list of some important messages is provided below. 


```
roadCameraState # frameID and image
liveParameters # hardcoded kalman filter
controlsState # various notifications
liveTracks # tracks for debugging purposes
liveCalibration # RPY values based on odometry
```

The magic of OpenPilot happens inside `selfdrive/modeld/models/driving.cc` where the model creates multiple plans based on the current driving scenario. The model is executed based on a `.dlc` container file probably proprietary to Qualcomm.

To be continued ...