# Classification

- tensor-group: no
- layer-type: output
- use-case: pose-estimation

# Description

Combined pose estimation predictions containing bounding boxes, person confidence scores,
and keypoint coordinates for YOLOv8 pose models exported with older Ultralytics versions
that do not embed stride multiplication in the ONNX detect head.

This tensor has the same structure as [yolo-v8-pose-out](/tensors/yolo-v8-pose-out.md) except that
bounding box coordinates (X, Y, W, H) and keypoint coordinates (KX, KY) are **normalized to [0, 1]**
relative to the input image dimensions, rather than expressed in pixel space. Models exported with
Ultralytics prior to the stride-folding change produce this encoding.

## Pose Predictions Tensor

- tensor-shape: BATCH_SIZE x 56 x NUM_CANDIDATES
- tensor-datatype: float32
- tensor-id: yolo-v8-pose-out-normalized
- memory-layout: column-major order

Where:
 * BATCH_SIZE is the size of the batch
 * 56 = 4 (bbox) + 1 (score) + 17×3 (keypoints: x, y, confidence)
 * NUM_CANDIDATES is the total number of candidates

### Known Aliases
* output0

### Encoding

Scheme: [X, Y, W, H, S, K1X, K1Y, K1C, ..., K17X, K17Y, K17C]

- X, Y = box center (normalized to [0, 1])
- W, H = box width/height (normalized to [0, 1])
- S = person-confidence score
- KkX, KkY = keypoint k coordinates (normalized to [0, 1]), k = 1...17
- KkC = keypoint k confidence score, k = 1...17

To obtain pixel coordinates multiply X, Y, W, H, KkX, KkY by the input image width/height.

Memory layout of tensor data:

| Index                               | Symbol | Value                           | Comment                                         |
|-------------------------------------|--------|---------------------------------|-------------------------------------------------|
| 0                                   | X      | box-0-x-center                  | tensor-start, x-center-plane-start              |
| ...                                 | ...    | ...                             | ...                                             |
| NUM_CANDIDATES × 1 - 1             | X      | box-C-x-center                  | tensor-continue, x-center-plane-end             |
| NUM_CANDIDATES × 1                  | Y      | box-0-y-center                  | tensor-continue, y-center-plane-start           |
| ...                                 | ...    | ...                             | ...                                             |
| NUM_CANDIDATES × 2 - 1             | Y      | box-C-y-center                  | tensor-continue, y-center-plane-end             |
| NUM_CANDIDATES × 2                  | W      | box-0-width                     | tensor-continue, width-plane-start              |
| ...                                 | ...    | ...                             | ...                                             |
| NUM_CANDIDATES × 3 - 1             | W      | box-C-width                     | tensor-continue, width-plane-end                |
| NUM_CANDIDATES × 3                  | H      | box-0-height                    | tensor-continue, height-plane-start             |
| ...                                 | ...    | ...                             | ...                                             |
| NUM_CANDIDATES × 4 - 1             | H      | box-C-height                    | tensor-continue, height-plane-end               |
| NUM_CANDIDATES × 4                  | S      | box-0-confidence-score          | tensor-continue, confidence-score-plane-start   |
| ...                                 | ...    | ...                             | ...                                             |
| NUM_CANDIDATES × 5 - 1             | S      | box-C-confidence-score          | tensor-continue, confidence-score-plane-end     |
| NUM_CANDIDATES × 5                  | K1X    | box-0-keypoint-1-x-coord        | tensor-continue, keypoint-1-x-plane-start       |
| ...                                 | ...    | ...                             | ...                                             |
| NUM_CANDIDATES × 6 - 1             | K1X    | box-C-keypoint-1-x-coord        | tensor-continue, keypoint-1-x-plane-end         |
| NUM_CANDIDATES × 6                  | K1Y    | box-0-keypoint-1-y-coord        | tensor-continue, keypoint-1-y-plane-start       |
| ...                                 | ...    | ...                             | ...                                             |
| NUM_CANDIDATES × 7 - 1             | K1Y    | box-C-keypoint-1-y-coord        | tensor-continue, keypoint-1-y-plane-end         |
| NUM_CANDIDATES × 7                  | K1C    | box-0-keypoint-1-confidence     | tensor-continue, keypoint-1-confidence-plane-start |
| ...                                 | ...    | ...                             | ...                                             |
| NUM_CANDIDATES × 8 - 1             | K1C    | box-C-keypoint-1-confidence     | tensor-continue, keypoint-1-confidence-plane-end   |
| ...                                 | ...    | ...                             | ...                                             |
| NUM_CANDIDATES × 53                 | K17X   | box-0-keypoint-17-x-coord       | tensor-continue, keypoint-17-x-plane-start      |
| ...                                 | ...    | ...                             | ...                                             |
| NUM_CANDIDATES × 54 - 1            | K17X   | box-C-keypoint-17-x-coord       | tensor-continue, keypoint-17-x-plane-end        |
| NUM_CANDIDATES × 54                 | K17Y   | box-0-keypoint-17-y-coord       | tensor-continue, keypoint-17-y-plane-start      |
| ...                                 | ...    | ...                             | ...                                             |
| NUM_CANDIDATES × 55 - 1            | K17Y   | box-C-keypoint-17-y-coord       | tensor-continue, keypoint-17-y-plane-end        |
| NUM_CANDIDATES × 55                 | K17C   | box-0-keypoint-17-confidence    | tensor-continue, keypoint-17-confidence-plane-start |
| ...                                 | ...    | ...                             | ...                                             |
| NUM_CANDIDATES × 56 - 1            | K17C   | box-C-keypoint-17-confidence    | tensor-end, keypoint-17-confidence-plane-end    |

# External References

* [YOLOv8 Paper](https://arxiv.org/abs/2305.09972)
* [Ultralytics YOLOv8 Documentation](https://docs.ultralytics.com/models/yolov8/)
* [Ultralytics ONNX export source](https://github.com/ultralytics/ultralytics/blob/main/ultralytics/engine/exporter.py)

# Models

* [YOLOv8s-pose (old Ultralytics export)](https://github.com/ultralytics/assets/releases)

# Tensor Decoders

|Framework  | Links |
|---        |---    |
|GStreamer  | [yolov8tensordec](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/tree/main/subprojects/gst-plugins-bad/gst/tensordecoders) |
