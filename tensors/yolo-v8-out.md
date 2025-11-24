# Classification

- tensor-group: no
- layer-type: output
- use-case: object-detection

# Description

Combined detection predictions containing bounding boxes and class probabilities YOLO v8-v11 ibject detection models model.

This is the base for the [YOLOv8 Segmentation detections tensor](/tensors/yolo-v8-segmentation-out-detections.md).

## Detection Predictions Tensor

- tensor-shape: BATCH_SIZE x (4 + NUM_CLASSES) x NUM_CANDIDATES)
- tensor-datatype: float32
- memory-layout: column-major order

Where:
 * BATCH_SIZE is the size of the batch
 * NUM_CLASSES is the number the classes the model was train
 * NUM_CANDIDATES is the total number of candidates

### Known Aliases
* output0


### Encoding

Scheme: (X: x-coord, Y: y-coord, W: width, H: height, C0-Cx: class probabilities)

The tensor contains 116 channels with the following layout per spatial location:

|Channels 0-3  | Channels 4-x |
|---           |---           |
| Bounding Box | Class Probs  |
| [X,Y,W,H]    | [C0...Cx]    |

Memory layout of tensor data (for spatial location i):

Memory layout of tensor data:

|Index                         | Symbol            | Value                    | Comment                                    |
|---                           |---                |---                       |---                                         |
| -                            | -                 | -                        | -                                          |
|0                             | X                 | box-0-x-center           | tensor-start, x-center-plane-start        |
|...                           | ...               | ...                      | ...                                        |
|NUM_CANDIDATES × 1 - 1        | X                 | box-C-x-center       | tensor-continue, x-center-plane-end       |
|NUM_CANDIDATES × 1            | Y                 | box-C-y-center           | tensor-continue, y-center-plane-start     |
|...                           | ...               | ...                      | ...                                        |
|NUM_CANDIDATES × 2 - 1        | Y                 | box-C-y-center       | tensor-continue, y-center-plane-end       |
|NUM_CANDIDATES × 2            | W                 | box-C-width              | tensor-continue, width-plane-start        |
|...                           | ...               | ...                      | ...                                        |
|NUM_CANDIDATES × 3 - 1        | W                 | box-C-width          | tensor-continue, width-plane-end          |
|NUM_CANDIDATES × 3            | H                 | box-C-height             | tensor-continue, height-plane-start       |
|...                           | ...               | ...                      | ...                                        |
|NUM_CANDIDATES × 4 - 1        | H                 | box-C-height         | tensor-continue, height-plane-end         |
|NUM_CANDIDATES × 4            | C0                | box-C-class-0-prob       | tensor-continue, class-0-plane-start      |
|...                           | ...               | ...                      | ...                                        |
|NUM_CANDIDATES × 5 - 1        | C0                | box-C-class-0-prob   | tensor-continue, class-0-plane-end        |
|NUM_CANDIDATES × 5            | C1                | box-C-class-1-prob       | tensor-continue, class-1-plane-start      |
|...                           | ...               | ...                      | ...                                        |
|NUM_CANDIDATES × 6 - 1        | C1                | box-C-class-1-prob   | tensor-continue, class-1-plane-end        |
|...                           | ...               | ...                      | ...                                        |
|NUM_CANDIDATES × (NUM_CLASSES + 4) - 1 | Cx                | box-C-class-c-prob       | tensor-continue, class--plane-start     |
|...                                    | ...               | ...                      | ...                                        |
|NUM_CANDIDATES × (NUM_CLASSES + 4) - 1 | Cx                | box-C-class-c-prob       | tensor-end, class--plane-end     |

# External References

* [YOLOv8 Paper](https://arxiv.org/abs/2305.09972)
* [Ultralytics YOLOv5 Documentation](https://docs.ultralytics.com/models/yolov5/)
* [Ultralytics YOLOv8 Documentation](https://docs.ultralytics.com/models/yolov8/)
* [Ultralytics YOLOv11 Documentation](https://docs.ultralytics.com/models/yolov11/)

