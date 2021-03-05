---
title:  "Comma Docs"
mathjax: true
layout: post
categories: machine-learning
---

OpenPilot from comma.ai heavily relies on multiple daemons that interact via message passing with each other. The core services can be found inside `selfdrive/` and `tools/`. `selfdrive/test/model_replay.py` provides a test to playback messages that were previously recorded. `tools/` contains further examples to test your current model. Messages for IPC are sent via comma.ai's own [cereal framework](https://github.com/commaai/cereal). Cereal is built ontop of capnp and therefore also has a very low overhead. A list of OpenPilot messages is provided below. 


```
roadCameraState # frameID and image
liveParameters # hardcoded kalman filter
controlsState # various notifications
liveTracks # tracks for debugging purposes
liveCalibration # RPY values based on odometry
```

The magic of OpenPilot happens inside `selfdrive/modeld/models/driving.cc` where the model creates multiple plans based on the current driving scenario. The model is executed based on a `.dlc` container file proprietary to Qualcomm. `selfdrive/modeld/thneed` contains some accelerator stuff for model loading. Apart from `.dlc`, OpenPilot also provides Keras and ONNX models inside the `models/` directory. Displaying the model in Netron provides some insights into the structure of the model. `supercombo.onnx` combines multiple models via a concatenation function at the output. Apart from the 4 previous frames the child models take a desire, traffic conventions and previous temporal outputs as input. The input frames are passed to the network in a YUV encoding and constantly moved up in the buffer. Transformations before passing the input frames to the network take place in the `frame_prepare()` function inside `selfdrive/models/commonmodel.cc`. This function calls `transform_queue()` and `loadyuv_queue()`. The output size is halved due to debayering of the original image. The output of `supercombo.onnx` is a one dimensional tensor with 11337 elements. The indexing of this tensor can be reconstructed from `selfdrive/modeld/models/driving.cc`.

Since multiple processes and threads are executed concurrently in OpenPilot we need to make sure that resources are properly accessed. For transforms `pthread_mutex_lock()` is used to avoid manipulation of the transformation matrix during matrix multiplications. Scheduling priorities of processes are set via a wrapped `os.sched_setscheduler()` call that is called `set_realtime_priority()`.

To accelearte various operations OpenPilot uses a GPU via OpenCL. The projects is created using the `scons` build tool (comparable to `cmake`).


To be continued ...