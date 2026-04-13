# Classification

- tensor-group: no
- layer-type: output
- use-case: hand-landmark
- part-of-tensor-groups:
    - [hand-landmark-out](/tensor-groups/hand-landmark-out.md)

# Description

Hand landmark keypoint coordinates from a hand pose estimation model. Contains
the (x, y, z) or (x, y) coordinates of 21 hand keypoints.

## Landmarks Tensor

- tensor-shape: N x 63 (flattened 21 keypoints × 3 coordinates) or N x 21 x stride
- tensor-datatype: float32

Where:
 * N is the number of detected hands
 * stride is typically 3 (x, y, z) or 2 (x, y)

### Known Aliases
* xyz_x21 (ONNX model output name)
* hand_landmarks (used by GStreamer handlandmarktensordec element)

### Encoding

21 keypoints in MediaPipe hand landmark order:

| Index | Landmark         |
|-------|-----------------|
| 0     | Wrist            |
| 1     | Thumb CMC        |
| 2     | Thumb MCP        |
| 3     | Thumb IP         |
| 4     | Thumb Tip        |
| 5     | Index MCP        |
| 6     | Index PIP        |
| 7     | Index DIP        |
| 8     | Index Tip        |
| 9     | Middle MCP       |
| 10    | Middle PIP       |
| 11    | Middle DIP       |
| 12    | Middle Tip       |
| 13    | Ring MCP         |
| 14    | Ring PIP         |
| 15    | Ring DIP         |
| 16    | Ring Tip         |
| 17    | Pinky MCP        |
| 18    | Pinky PIP        |
| 19    | Pinky DIP        |
| 20    | Pinky Tip        |

Each keypoint is encoded as (x, y, z) where x and y are in normalized image
coordinates [0, 1] and z is relative depth.

# External References

* [MediaPipe Hand Landmark Model](https://google.github.io/mediapipe/solutions/hands.html)
* [Hand Gesture Recognition Using ONNX](https://github.com/PINTO0309/hand-gesture-recognition-using-onnx)

# Models

* [hand_landmark_sparse_Nx3x224x224.onnx](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/hand_landmark_sparse_Nx3x224x224.onnx)
* [hand_landmark_sparse_Nx3x224x224.onnx](https://github.com/PINTO0309/hand-gesture-recognition-using-onnx) (upstream source)

# Tensor Decoders

|Framework | Links |
|---       |---    |
|ONNX    | [GStreamer handlandmarktensordec](https://gitlab.freedesktop.org/gstreamer/gst-plugins-rs) |
