# Classification

- tensor-group: no
- layer-type: output
- use-case: oriented-object-detection

# Description

Combined detection predictions containing bounding boxes, class probabilities, and
rotation angle for YOLOv8 OBB model.

This is based on [YOLO v8](/tensors/yolo-v8-out.md)

## Detection Predictions Tensor

- tensor-shape: BATCH_SIZE x (4 + NUM_CLASSES + 1) x NUM_CANDIDATES
- tensor-datatype: float32
- memory-layout: column-major order

Where:
 * BATCH_SIZE is the size of the batch
 * NUM_CLASSES is the number of classes the model was trained on
 * NUM_CANDIDATES is the total number of candidates

### Known Aliases
* output0


### Encoding

Scheme: (X: x-coord, Y: y-coord, W: width, H: height, C0-Cc: class probabilities, R: rotation)

The tensor contains (4 + NUM_CLASSES + 1) channels with the following layout per spatial location:

| Channels 0-3 | Channels 4-(NUM_CLASSES+3) | Channel NUM_CLASSES+4 |
|--------------|----------------------------|-----------------------|
| Bounding Box | Class Probs                | Rotation Angle        |
| [X,Y,W,H]    | [C0...Cc]                  | [R]                   |

Memory layout of tensor data (for spatial location i):

| Index                                  | Symbol | Value              | Comment                               |
|----------------------------------------|--------|--------------------|---------------------------------------|
| 0                                      | X      | box-0-x-center     | tensor-start, x-center-plane-start    |
| ...                                    | ...    | ...                | ...                                   |
| NUM_CANDIDATES × 1 - 1                 | X      | box-C-x-center     | tensor-continue, x-center-plane-end   |
| NUM_CANDIDATES × 1                     | Y      | box-0-y-center     | tensor-continue, y-center-plane-start |
| ...                                    | ...    | ...                | ...                                   |
| NUM_CANDIDATES × 2 - 1                 | Y      | box-C-y-center     | tensor-continue, y-center-plane-end   |
| NUM_CANDIDATES × 2                     | W      | box-0-width        | tensor-continue, width-plane-start    |
| ...                                    | ...    | ...                | ...                                   |
| NUM_CANDIDATES × 3 - 1                 | W      | box-C-width        | tensor-continue, width-plane-end      |
| NUM_CANDIDATES × 3                     | H      | box-0-height       | tensor-continue, height-plane-start   |
| ...                                    | ...    | ...                | ...                                   |
| NUM_CANDIDATES × 4 - 1                 | H      | box-C-height       | tensor-continue, height-plane-end     |
| NUM_CANDIDATES × 4                     | C0     | box-0-class-0-prob | tensor-continue, class-0-plane-start  |
| ...                                    | ...    | ...                | ...                                   |
| NUM_CANDIDATES × 5 - 1                 | C0     | box-C-class-0-prob | tensor-continue, class-0-plane-end    |
| NUM_CANDIDATES × 5                     | C1     | box-0-class-1-prob | tensor-continue, class-1-plane-start  |
| ...                                    | ...    | ...                | ...                                   |
| NUM_CANDIDATES × 6 - 1                 | C1     | box-C-class-1-prob | tensor-continue, class-1-plane-end    |
| ...                                    | ...    | ...                | ...                                   |
| NUM_CANDIDATES × (NUM_CLASSES + 3)     | Cc     | box-0-class-c-prob | tensor-continue, class-c-plane-start  |
| ...                                    | ...    | ...                | ...                                   |
| NUM_CANDIDATES × (NUM_CLASSES + 4) - 1 | Cc     | box-C-class-c-prob | tensor-continue, class-c-plane-end    |
| NUM_CANDIDATES × (NUM_CLASSES + 4)     | R      | box-0-rotation     | tensor-continue, rotation-plane-start |
| ...                                    | ...    | ...                | ...                                   |
| NUM_CANDIDATES × (NUM_CLASSES + 5) - 1 | R      | box-C-rotation     | tensor-end, rotation-plane-end        |



# External References

* [YOLOv8 Paper](https://arxiv.org/abs/2305.09972)
* [Ultralytics YOLOv8 Documentation](https://docs.ultralytics.com/models/yolov8/)

# Models

* [YOLOv8n-obb ONNX trained on DOTAv1](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/yolov8n-obb.onnx)
