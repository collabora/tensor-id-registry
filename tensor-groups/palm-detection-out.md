# Classification

- tensor-group: yes
- layer-type: output
- use-case: hand-detection

# Description

Palm Detection model outputs. This tensor group contains detection results for identifying palm/hand regions in images. The Palm Detection model is designed to locate hand bounding boxes and keypoints within an image.
Note this is a variation of the mediapipe hand detection model where only the wrist keypoint and middle finger joint to palm keypoint are used to detect hand orientation. 


Palm Detection Output Tensors:

| Name                | shape    |
|---                  |---       |
| [detections]        | N x 8    |

Where N is the number of detected palms/hands.

# Tensor Decoding Logic

```
Foreach detection in detections:
    score = detection[0]
    box_x = detection[1]
    box_y = detection[2]
    box_size = detection[3]
    kp0_x = detection[4]
    kp0_y = detection[5]
    kp2_x = detection[6]
    kp2_y = detection[7]

    # Filter by confidence threshold (typically 0.60)
    if score > confidence_threshold:
        output_detection = [score, box_x, box_y, box_size, kp0_x, kp0_y, kp2_x, kp2_y]
```

## Calculate the orientation of the bounding box

```
d_x = kp2_x - kp0_x
d_y = kp2_y - kp0_y

angle = atan2 (-d_y, d_x )
```
**d_y** is negated because, in image coordinate systems, the y-axis typically increases downward, opposite to the conventional mathematical coordinate system.
**angle** is measured in radians.

# External References

* [Hand Gesture Recognition Using ONNX](https://github.com/PINTO0309/hand-gesture-recognition-using-onnx)
* [MediaPipe Hand Detection](https://developers.google.com/edge/mediapipe/solutions/vision/hand_landmarker)
* [MediaPipe Hands: On-device Real-time Hand Tracking](https://arxiv.org/pdf/2006.10214)

# Models

* [palm_detection_full_inf_post_192x192.onnx](https://gitlab.collabora.com/gstreamer/ml-models/-/blob/b2c82c14394765e25286d6273f9fe01ddfed744f/models/palm_detection_full_inf_post_192x192.onnx)

# Tensor Decoders

|Framework | Links |
|---       |---    |
|ONNX / Python | [Hand gesture recognition using ONNX](https://github.com/PINTO0309/hand-gesture-recognition-using-onnx) |
|GStreamer | [handdetectiontensordec](https://gitlab.freedesktop.org/gstreamer/gst-plugins-rs/-/tree/0889cf6414cd5250c19626a600feff5cb15ac6ae/analytics/analytics/src/hand/handdetectiontensordec)|


[detections]: /tensors/palm-detection-out.md
