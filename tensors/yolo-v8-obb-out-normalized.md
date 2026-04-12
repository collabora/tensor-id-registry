# Classification

- tensor-group: no
- layer-type: output
- use-case: oriented-object-detection

# Description

Combined oriented bounding box detection predictions containing bounding boxes, class
probabilities, and rotation angle for YOLOv8 OBB models exported with older Ultralytics
versions that do not embed stride multiplication in the ONNX detect head.

This tensor has the same structure as [yolo-v8-obb-out](/tensors/yolo-v8-obb-out.md) except that
bounding box coordinates (X, Y, W, H) are **normalized to [0, 1]** relative to the input image
dimensions, rather than expressed in pixel space. The rotation angle R is unchanged. Models
exported with Ultralytics prior to the stride-folding change produce this encoding.

## Detection Predictions Tensor

- tensor-shape: BATCH_SIZE x (4 + NUM_CLASSES + 1) x NUM_CANDIDATES
- tensor-datatype: float32
- tensor-id: yolo-v8-obb-out-normalized
- memory-layout: column-major order

Where:
 * BATCH_SIZE is the size of the batch
 * NUM_CLASSES is the number of classes the model was trained on
 * NUM_CANDIDATES is the total number of candidates

### Known Aliases
* output0

### Encoding

Scheme: (X: x-coord, Y: y-coord, W: width, H: height, C0-Cc: class probabilities, R: rotation)

Bounding box fields X, Y, W, H are **center-format** and **normalized** to the input image
resolution (values in [0, 1]). To obtain pixel coordinates multiply by the input image
width/height. The rotation angle R is in radians and is not normalized.

Memory layout of tensor data:

| Index                                   | Symbol | Value               | Comment                                |
|-----------------------------------------|--------|---------------------|----------------------------------------|
| 0                                       | X      | box-0-x-center      | tensor-start, x-center-plane-start     |
| ...                                     | ...    | ...                 | ...                                    |
| NUM_CANDIDATES × 1 - 1                  | X      | box-C-x-center      | tensor-continue, x-center-plane-end    |
| NUM_CANDIDATES × 1                      | Y      | box-0-y-center      | tensor-continue, y-center-plane-start  |
| ...                                     | ...    | ...                 | ...                                    |
| NUM_CANDIDATES × 2 - 1                  | Y      | box-C-y-center      | tensor-continue, y-center-plane-end    |
| NUM_CANDIDATES × 2                      | W      | box-0-width         | tensor-continue, width-plane-start     |
| ...                                     | ...    | ...                 | ...                                    |
| NUM_CANDIDATES × 3 - 1                  | W      | box-C-width         | tensor-continue, width-plane-end       |
| NUM_CANDIDATES × 3                      | H      | box-0-height        | tensor-continue, height-plane-start    |
| ...                                     | ...    | ...                 | ...                                    |
| NUM_CANDIDATES × 4 - 1                  | H      | box-C-height        | tensor-continue, height-plane-end      |
| NUM_CANDIDATES × 4                      | C0     | box-0-class-0-prob  | tensor-continue, class-0-plane-start   |
| ...                                     | ...    | ...                 | ...                                    |
| NUM_CANDIDATES × 5 - 1                  | C0     | box-C-class-0-prob  | tensor-continue, class-0-plane-end     |
| NUM_CANDIDATES × 5                      | C1     | box-0-class-1-prob  | tensor-continue, class-1-plane-start   |
| ...                                     | ...    | ...                 | ...                                    |
| NUM_CANDIDATES × 6 - 1                  | C1     | box-C-class-1-prob  | tensor-continue, class-1-plane-end     |
| ...                                     | ...    | ...                 | ...                                    |
| NUM_CANDIDATES × (NUM_CLASSES + 3)      | Cc     | box-0-class-c-prob  | tensor-continue, class-c-plane-start   |
| ...                                     | ...    | ...                 | ...                                    |
| NUM_CANDIDATES × (NUM_CLASSES + 4) - 1  | Cc     | box-C-class-c-prob  | tensor-continue, class-c-plane-end     |
| NUM_CANDIDATES × (NUM_CLASSES + 4)      | R      | box-0-rotation      | tensor-continue, rotation-plane-start  |
| ...                                     | ...    | ...                 | ...                                    |
| NUM_CANDIDATES × (NUM_CLASSES + 5) - 1  | R      | box-C-rotation      | tensor-end, rotation-plane-end         |

# External References

* [YOLOv8 Paper](https://arxiv.org/abs/2305.09972)
* [Ultralytics YOLOv8 Documentation](https://docs.ultralytics.com/models/yolov8/)
* [Ultralytics ONNX export source](https://github.com/ultralytics/ultralytics/blob/main/ultralytics/engine/exporter.py)

# Models

* [YOLOv8n-obb (old Ultralytics export)](https://github.com/ultralytics/assets/releases)

# Tensor Decoders

|Framework  | Links |
|---        |---    |
|GStreamer  | [yolov8tensordec](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/tree/main/subprojects/gst-plugins-bad/gst/tensordecoders) |
