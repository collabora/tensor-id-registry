# Classification

- tensor-group: yes
- layer-type: output
- use-case: hand-detection

# Description

Palm Detection model outputs. This tensor group contains detection results for identifying palm/hand regions in images. The Palm Detection model is designed to locate hand bounding boxes and keypoints within an image.

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

* [MediaPipe Hand Detection](https://google.github.io/mediapipe/solutions/hands.html)
* [Palm Detection Model Architecture](https://github.com/google/mediapipe/blob/master/mediapipe/modules/hand_landmark/hand_detection.pbtxt)
* [Hand Gesture Recognition Using ONNX](https://github.com/PINTO0309/hand-gesture-recognition-using-onnx)

# Models

* [palm_detection_full_inf_post_192x192.onnx]

# Tensor Decoders

|Framework | Links |
|---       |---    |
|ONNX / Python | [Hand gesture recognition using ONNX](https://github.com/PINTO0309/hand-gesture-recognition-using-onnx) |


[detections]: /tensors/palm-detection-out-detections.md
[palm_detection_full_inf_post_192x192.onnx]: /models/palm_detection_full_inf_post_192x192.onnx
