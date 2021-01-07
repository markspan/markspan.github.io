---
layout: posts
title: "VideoCapture"
tags: labstreaminglayer video synchronisation coding
---

[VideoCapture](https://github.com/markspan/VideoCapture/blob/master/README.md)

Program to retrieve a video stream from a OpenCV compatible webcam and save it to a file in MJPEG format. Simultaneously creating an [labstreaminglayer](https://github.com/sccn/labstreaminglayer) Marker stream to create timestamps for each retrieved frame.

![VideoCaptureConfig](/images/VideoCapture-config.png)

For each discovered camera on the system, you can enter a filename and a stream-name. The filename will contain the recorded video, and will be recorded to the directory provided. Make sure this directory is writeable.
The stream-name is the name with which the stream will be presented to LSL, and this stream needs to be recorded separately with a program capabler of doing so. Usually the [Labrecorder](https://github.com/labstreaminglayer/App-LabRecorder) will be used.

The marker stream can be used to obtain the timestamps for each frame in each video file.
