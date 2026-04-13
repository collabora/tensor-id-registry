# Classification

- tensor-group: yes
- layer-type: output
- use-case: hand-landmark

# Description

Hand landmark model outputs. This tensor group contains keypoint coordinates,
confidence score, and handedness for one or more detected hands.

Hand Landmark Output Tensors:

| Name                        | Shape       |
|---                          |---          |
| [landmarks]                 | N x 63      |
| [score]                     | N x 1       |
| [handedness]                | N x 1       |

Where N is the number of detected hands.

# Tensor Decoding Logic

```
Foreach hand in batch:
    score = score_tensor[hand]

    # Filter by confidence threshold (typically 0.5)
    if score < confidence_threshold:
        continue

    landmarks = landmark_tensor[hand]  # 21 keypoints x stride
    handedness = handedness_tensor[hand]  # 0 = left, 1 = right

    Foreach keypoint_idx in 0..21:
        x = landmarks[keypoint_idx * stride + 0]
        y = landmarks[keypoint_idx * stride + 1]
        z = landmarks[keypoint_idx * stride + 2]  # if stride >= 3
```

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


[landmarks]: /tensors/hand-landmark-out-landmarks.md
[score]: /tensors/hand-landmark-out-score.md
[handedness]: /tensors/hand-landmark-out-handedness.md
