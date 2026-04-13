# Classification

- tensor-group: no
- layer-type: output
- use-case: hand-landmark
- part-of-tensor-groups:
    - [hand-landmark-out](/tensor-groups/hand-landmark-out.md)

# Description

Hand detection confidence score from a hand pose estimation model. Indicates
how confident the model is that a hand is present in the input crop.

## Score Tensor

- tensor-shape: N x 1
- tensor-datatype: float32

Where:
 * N is the number of detected hands

### Known Aliases
* hand_score (used by GStreamer handlandmarktensordec element)

### Encoding

A single float per hand in range [0, 1] where higher values indicate greater
confidence that a hand is present.

### Details

- **hand_score**: Confidence score for the hand detection. Typically filtered
  with a threshold (default 0.5 in the GStreamer element) to suppress
  low-confidence detections.

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
