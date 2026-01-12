# Classification

- tensor-group: no
- layer-type: output
- use-case: hand-detection
- part-of-tensor-groups:
    - [palm-detection-out](/tensor-groups/palm-detection-out.md)

# Description

Palm detection predictions containing bounding box location, confidence score, and hand keypoints from the Palm Detection model. This tensor contains detection results for identifying palm/hand regions in images.
Note this is a variation of the mediapipe hand detection model where only the wrist keypoint and middle finger joint to palm keypoint are used to detect hand orientation. 

## Detections Tensor

- tensor-shape: N x 8
- tensor-datatype: float32
- tensor-id: palm-detection-out
- memory-layout: row-major order

Where:
 * N is the number of detected palms/hands

### Known Aliases
* pdscore_boxx_boxy_boxsize_kp0x_kp0y_kp2x_kp2y

### Encoding

Scheme: (Score, Box_X, Box_Y, Box_Size, Keypoint0_X, Keypoint0_Y, Keypoint2_X, Keypoint2_Y)

The tensor contains 8 channels with the following layout per detection:

|Channel 0  | Channel 1 | Channel 2 | Channel 3  | Channel 4 | Channel 5 | Channel 6 | Channel 7 |
|---        |---        |---        |---         |---        | ---       |---        | ---  |
| Score     | Box_X     | Box_Y     | Box_Size   | Keypoint0-x   | Keypoint0-y | Keypoint2-x | Keypoint2-y   |


Memory layout of tensor data (for detection i):

|Index           | Symbol            | Value              | Comment                                    |
|---             |---                |---                 |---                                         |
|0               | pd_score          | detection-i-confidence | tensor-start,            |
|1               | box_x             | detection-i-box-center-x | tensor-continue,         |
|2               | box_y             | detection-i-box-center-y | tensor-continue,         |
|3               | box_size          | detection-i-size   | tensor-continue,          |
|4               | keypoint0_x             | detection-i-keypoint0-x | tensor-continue,    |
|5               | keypoint0_y             | detection-i-keypoint0-y | tensor-continue,    |
|6               | keypoint2_x             | detection-i-keypoint2-x | tensor-continue,    |
|7               | keypoint2_y             | detection-i-keypoint2-y | tensor-end,        |

### Details

- **pd_score** (Channel 0): Confidence score for the detection, in range [0, 1]
- **box_x, box_y** (Channels 1-2): Center coordinates of the detected palm bounding box in range [0,1] where 0 corresponds to the top (or left) and 1 to bottom (or right) edge of the image
- **box_size** (Channel 3): Side length of the bounding box, where the bounding box is square
- **Keypoint0** (Channels 4-5): Wrist keypoint 
- **Keypoint2** (Channels 6-7): Middle finger joint to palm


# External References

* [MediaPipe Hand Detection](https://developers.google.com/edge/mediapipe/solutions/vision/hand_landmarker)
* [MediaPipe Hands: On-device Real-time Hand Tracking](https://arxiv.org/pdf/2006.10214)
* [Hand gesture recognition using ONNX](https://github.com/PINTO0309/hand-gesture-recognition-using-onnx/blob/main/model/palm_detection/palm_detection.py)

# Models

* [palm_detection_full_inf_post_192x192.onnx](https://gitlab.collabora.com/gstreamer/ml-models/-/blob/b2c82c14394765e25286d6273f9fe01ddfed744f/models/palm_detection_full_inf_post_192x192.onnx)

# Tensor Decoders

|Framework | Links |
|---       |---    |
|ONNX    | [Hand gesture recognition using ONNX](https://github.com/PINTO0309/hand-gesture-recognition-using-onnx/blob/main/model/palm_detection/palm_detection.py) |
|GStreamer | [handdetectiontensordec](https://gitlab.freedesktop.org/gstreamer/gst-plugins-rs/-/tree/0889cf6414cd5250c19626a600feff5cb15ac6ae/analytics/analytics/src/hand/handdetectiontensordec)|



