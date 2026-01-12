# Classification

- tensor-group: no
- layer-type: output
- use-case: hand-detection
- part-of-tensor-groups:
    - [palm-detection-out](/tensor-groups/palm-detection-out.md)

# Description

Palm detection predictions containing bounding box location, confidence score, and hand keypoints from the Palm Detection model. This tensor contains detection results for identifying palm/hand regions in images.

## Detections Tensor

- tensor-shape: N x 8
- tensor-datatype: float32

Where:
 * N is the number of detected palms/hands

### Known Aliases
* output0

### Encoding

Scheme: (Score, Box_X, Box_Y, Box_Size, Keypoint0_X, Keypoint0_Y, Keypoint1_X, Keypoint1_Y)

The tensor contains 8 channels with the following layout per detection:

|Channel 0  | Channel 1 | Channel 2 | Channel 3  | Channel 4-5 | Channel 6-7 |
|---        |---        |---        |---         |---          |---          |
| Score     | Box_X     | Box_Y     | Box_Size   | Keypoint0   | Keypoint1   |
| pd_score  | box_x     | box_y     | box_size   | (x, y)      | (x, y)      |

Memory layout of tensor data (for detection i):

|Index           | Symbol            | Value              | Comment                                    |
|---             |---                |---                 |---                                         |
| -              | -                 | -                  | -                                          |
|0               | pd_score          | detection-i-confidence | tensor-start, detection-i-score            |
|1               | box_x             | detection-i-box-center-x | tensor-continue, detection-i-box-x         |
|2               | box_y             | detection-i-box-center-y | tensor-continue, detection-i-box-y         |
|3               | box_size          | detection-i-size   | tensor-continue, detection-i-size          |
|4               | kp0_x             | detection-i-keypoint0-x | tensor-continue, detection-i-keypoint0-x   |
|5               | kp0_y             | detection-i-keypoint0-y | tensor-continue, detection-i-keypoint0-y   |
|6               | kp2_x             | detection-i-keypoint2-x | tensor-continue, detection-i-keypoint2-x   |
|7               | kp2_y             | detection-i-keypoint2-y | tensor-end, detection-i-keypoint2-y        |

### Details

- **pd_score** (Channel 0): Confidence score for the detection, typically in range [0, 1]
- **box_x, box_y** (Channels 1-2): Center coordinates of the detected palm bounding box
- **box_size** (Channel 3): Size parameter of the bounding box
- **Keypoint0** (Channels 4-5): Wrist keypoint 
- **Keypoint1** (Channels 4-5): Middle finger joint to palm


# External References

* [MediaPipe Hand Detection](https://google.github.io/mediapipe/solutions/hands.html)
* [Palm Detection Model Architecture](https://github.com/google/mediapipe/blob/master/mediapipe/modules/hand_landmark/hand_detection.pbtxt)

# Models

* palm_detection_full_inf_post_192x192.onnx

# Tensor Decoders

|Framework | Links |
|---       |---    |
|Keras    | Hand gesture recognition using ONNX |
