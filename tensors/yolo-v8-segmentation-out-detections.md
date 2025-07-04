# Classification

- tensor-group: no
- layer-type: output
- use-case: instance-segmentation
- part-of-tensor-groups:
    - [yolo-v8-segmentation-output](/tensor-groups/yolo-v8-segmentation-output.md)

# Description

Combined detection predictions containing bounding boxes, class probabilities, and
mask coefficients for YOLOv8 segmentation model. This tensor provides all detection
information plus the coefficients needed to generate segmentation masks when
combined with [prototype masks](/tensors/yolo-v8-segmentation-out-protos.md).

## Detection Predictions Tensor

- tensor-shape: 1 x 116 x 21504
- tensor-datatype: float32

### Known Aliases
* output0


### Encoding

Scheme: (X: x-coord, Y: y-coord, W: width, H: height, C0-C79: class probabilities, M0-M31: mask coefficients)

The tensor contains 116 channels with the following layout per spatial location:

|Channels 0-3 | Channels 4-83 | Channels 84-115 |
|---          |---            |---              |
| Bounding Box | Class Probs   | Mask Coeffs     |
| [X,Y,W,H]   | [C0...C79]    | [M0...M31]      |

Memory layout of tensor data (for spatial location i):

Memory layout of tensor data:

|Index                          | Symbol            | Value                    | Comment                                    |
|---                           |---                |---                       |---                                         |
| -                            | -                 | -                        | -                                          |
|0                             | X                 | box-0-x-center           | tensor-start, x-center-plane-start        |
|...                           | ...               | ...                      | ...                                        |
|21504 × 1 - 1                 | X                 | box-21503-x-center       | tensor-continue, x-center-plane-end       |
|21504 × 1                     | Y                 | box-0-y-center           | tensor-continue, y-center-plane-start     |
|...                           | ...               | ...                      | ...                                        |
|21504 × 2 - 1                 | Y                 | box-21503-y-center       | tensor-continue, y-center-plane-end       |
|21504 × 2                     | W                 | box-0-width              | tensor-continue, width-plane-start        |
|...                           | ...               | ...                      | ...                                        |
|21504 × 3 - 1                 | W                 | box-21503-width          | tensor-continue, width-plane-end          |
|21504 × 3                     | H                 | box-0-height             | tensor-continue, height-plane-start       |
|...                           | ...               | ...                      | ...                                        |
|21504 × 4 - 1                 | H                 | box-21503-height         | tensor-continue, height-plane-end         |
|21504 × 4                     | C0                | box-0-class-0-prob       | tensor-continue, class-0-plane-start      |
|...                           | ...               | ...                      | ...                                        |
|21504 × 5 - 1                 | C0                | box-21503-class-0-prob   | tensor-continue, class-0-plane-end        |
|21504 × 5                     | C1                | box-0-class-1-prob       | tensor-continue, class-1-plane-start      |
|...                           | ...               | ...                      | ...                                        |
|21504 × 6 - 1                 | C1                | box-21503-class-1-prob   | tensor-continue, class-1-plane-end        |
|...                           | ...               | ...                      | ...                                        |
|21504 × 83                    | C79               | box-0-class-79-prob      | tensor-continue, class-79-plane-start     |
|...                           | ...               | ...                      | ...                                        |
|21504 × 84 - 1                | C79               | box-21503-class-79-prob  | tensor-continue, class-79-plane-end       |
|21504 × 84                    | M0                | box-0-mask-coeff-0       | tensor-continue, mask-coeff-0-plane-start |
|...                           | ...               | ...                      | ...                                        |
|21504 × 85 - 1                | M0                | box-21503-mask-coeff-0   | tensor-continue, mask-coeff-0-plane-end   |
|21504 × 85                    | M1                | box-0-mask-coeff-1       | tensor-continue, mask-coeff-1-plane-start |
|...                           | ...               | ...                      | ...                                        |
|21504 × 86 - 1                | M1                | box-21503-mask-coeff-1   | tensor-continue, mask-coeff-1-plane-end   |
|...                           | ...               | ...                      | ...                                        |
|21504 × 115                   | M31               | box-0-mask-coeff-31      | tensor-continue, mask-coeff-31-plane-start|
|...                           | ...               | ...                      | ...                                        |
|21504 × 116 - 1               | M31               | box-21503-mask-coeff-31  | tensor-end, mask-coeff-31-plane-end       |
| -                            | -                 | -                        | -                                          |


# External References

* [YOLOv8 Paper](https://arxiv.org/abs/2305.09972)
* [Ultralytics YOLOv8 Documentation](https://docs.ultralytics.com/models/yolov8/)

# Models

* [YOLOv8s-seg ONNX trained on COCO](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/yolov8s-seg.onnx)