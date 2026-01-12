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
* pdscore_boxx_boxy_boxsize_kp0x_kp0y_kp2x_kp2y

### Encoding

Scheme: (Score, Box_X, Box_Y, Box_Size, Keypoint0_X, Keypoint0_Y, Keypoint1_X, Keypoint1_Y)

The tensor contains 8 channels with the following layout per detection:

|Channel 0  | Channel 1 | Channel 2 | Channel 3  | Channel 4 | Channel 5 | Channel 6 | Channel 7 |
|---        |---        |---        |---         |---        | ---       |---        | ---  |
| Score     | Box_X     | Box_Y     | Box_Size   | Keypoint0-x   | Keypoint0-y | Keypoint1-x | Keypoint1-y   |


Memory layout of tensor data (for detection i):

|Index           | Symbol            | Value              | Comment                                    |
|---             |---                |---                 |---                                         |
| -              | -                 | -                  | -                                          |
|0               | pd_score          | detection-i-confidence | tensor-start, detection-i-score            |
|1               | box_x             | detection-i-box-center-x | tensor-continue, detection-i-box-x         |
|2               | box_y             | detection-i-box-center-y | tensor-continue, detection-i-box-y         |
|3               | box_size          | detection-i-size   | tensor-continue, detection-i-size          |
|4               | keypoint0_x             | detection-i-keypoint0-x | tensor-continue, detection-i-keypoint0-x   |
|5               | keypoint0_y             | detection-i-keypoint0-y | tensor-continue, detection-i-keypoint0-y   |
|6               | keypoint1_x             | detection-i-keypoint2-x | tensor-continue, detection-i-keypoint1-x   |
|7               | keypoinp1_y             | detection-i-keypoint2-y | tensor-end, detection-i-keypoint1-y        |

### Details

- **pd_score** (Channel 0): Confidence score for the detection, in range [0, 1]
- **box_x, box_y** (Channels 1-2): Center coordinates of the detected palm bounding box in range [0,1] where 0 corresponds to the top (or left) and 1 to bottom (or right) edge of the image
- **box_size** (Channel 3): Side length of the bounding box, where the bounding box is square
- **Keypoint0** (Channels 4-5): Wrist keypoint 
- **Keypoint1** (Channels 6-7): Middle finger joint to palm


# External References

* [MediaPipe Hand Detection](https://google.github.io/mediapipe/solutions/hands.html)
* [Palm Detection Model Architecture](https://github.com/google/mediapipe/blob/master/mediapipe/modules/hand_landmark/hand_detection.pbtxt)

# Models

* [palm_detection_full_inf_post_192x192.onnx](https://github.com/PINTO0309/hand-gesture-recognition-using-onnx/blob/main/model/palm_detection/palm_detection_full_inf_post_192x192.onnx)

# Tensor Decoders

|Framework | Links |
|---       |---    |
|ONNX    | [Hand gesture recognition using ONNX](https://github.com/PINTO0309/hand-gesture-recognition-using-onnx/blob/main/model/palm_detection/palm_detection.py) |
