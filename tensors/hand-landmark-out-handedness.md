# Classification

- tensor-group: no
- layer-type: output
- use-case: hand-landmark
- part-of-tensor-groups:
    - [hand-landmark-out](/tensor-groups/hand-landmark-out.md)

# Description

Handedness output for each detected hand. Indicates whether the model predicts
left hand or right hand.

## Handedness Tensor

- tensor-shape: N x 1
- tensor-datatype: float32

Where:
 * N is the number of detected hands

### Known Aliases
* lefthand_0_or_righthand_1 (ONNX model output name)
* hand_handedness

### Encoding

A single float per hand:

- `0.0` indicates left hand
- `1.0` indicates right hand

Some models may produce values close to 0 or 1 instead of exact integers.
In that case, a threshold at 0.5 can be used for classification.

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
