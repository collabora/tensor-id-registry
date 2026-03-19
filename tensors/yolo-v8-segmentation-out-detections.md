# Classification

- tensor-group: no
- layer-type: output
- use-case: instance-segmentation
- part-of-tensor-groups:
    - [yolo-v8-segmentation-out](/tensor-groups/yolo-v8-segmentation-out.md)

# Description

Combined detection predictions containing bounding boxes, class probabilities, and
mask coefficients for YOLOv8 segmentation model. This tensor provides all detection
information plus the coefficients needed to generate segmentation masks when
combined with [prototype masks](/tensors/yolo-v8-segmentation-out-protos.md).

This is based on [YOLO v8](/tensors/yolo-v8-out.md)

## Detection Predictions Tensor

- tensor-shape: BATCH_SIZE x (4 + NUM_CLASSES + NUM_MASKS) x NUM_CANDIDATES
- tensor-datatype: float32
- memory-layout: column-major order

Where:
 * BATCH_SIZE is the size of the batch
 * NUM_CLASSES is the number of classes the model was trained on
 * NUM_MASKS is the number of mask coefficients per detection
 * NUM_CANDIDATES is the total number of candidates, derived from the input image dimensions.
   YOLOv8 generates candidates from three feature map levels (strides 8, 16, 32):
   `NUM_CANDIDATES = (H/8 × W/8) + (H/16 × W/16) + (H/32 × W/32)`
   e.g. for a 640×640 input: `(80×80) + (40×40) + (20×20) = 8400`

### Known Aliases
* output0


### Encoding

Scheme: (X: x-coord, Y: y-coord, W: width, H: height, C0-Cc: class probabilities, M0-Mm: mask coefficients)

The tensor contains (4 + NUM_CLASSES + NUM_MASKS) channels with the following layout per spatial location:

|Channels 0-3  | Channels 4-(NUM_CLASSES+3)     | Channels (NUM_CLASSES+4)-(NUM_CLASSES+NUM_MASKS+3) |
|---           |---                             |---                                                 |
| Bounding Box | Class Probs                    | Mask Coeffs                                        |
| [X,Y,W,H]   | [C0...Cc]                      | [M0...Mm]                                          |

Memory layout of tensor data (for spatial location i):

Memory layout of tensor data:

|Index                                                    | Symbol | Value                  | Comment                                    |
|---                                                      |---     |---                     |---                                         |
| -                                                       | -      | -                      | -                                          |
|0                                                        | X      | box-0-x-center         | tensor-start, x-center-plane-start         |
|...                                                      | ...    | ...                    | ...                                        |
|NUM_CANDIDATES × 1 - 1                                   | X      | box-C-x-center         | tensor-continue, x-center-plane-end        |
|NUM_CANDIDATES × 1                                       | Y      | box-0-y-center         | tensor-continue, y-center-plane-start      |
|...                                                      | ...    | ...                    | ...                                        |
|NUM_CANDIDATES × 2 - 1                                   | Y      | box-C-y-center         | tensor-continue, y-center-plane-end        |
|NUM_CANDIDATES × 2                                       | W      | box-0-width            | tensor-continue, width-plane-start         |
|...                                                      | ...    | ...                    | ...                                        |
|NUM_CANDIDATES × 3 - 1                                   | W      | box-C-width            | tensor-continue, width-plane-end           |
|NUM_CANDIDATES × 3                                       | H      | box-0-height           | tensor-continue, height-plane-start        |
|...                                                      | ...    | ...                    | ...                                        |
|NUM_CANDIDATES × 4 - 1                                   | H      | box-C-height           | tensor-continue, height-plane-end          |
|NUM_CANDIDATES × 4                                       | C0     | box-0-class-0-prob     | tensor-continue, class-0-plane-start       |
|...                                                      | ...    | ...                    | ...                                        |
|NUM_CANDIDATES × 5 - 1                                   | C0     | box-C-class-0-prob     | tensor-continue, class-0-plane-end         |
|NUM_CANDIDATES × 5                                       | C1     | box-0-class-1-prob     | tensor-continue, class-1-plane-start       |
|...                                                      | ...    | ...                    | ...                                        |
|NUM_CANDIDATES × 6 - 1                                   | C1     | box-C-class-1-prob     | tensor-continue, class-1-plane-end         |
|...                                                      | ...    | ...                    | ...                                        |
|NUM_CANDIDATES × (NUM_CLASSES + 3)                       | Cc     | box-0-class-c-prob     | tensor-continue, class-c-plane-start       |
|...                                                      | ...    | ...                    | ...                                        |
|NUM_CANDIDATES × (NUM_CLASSES + 4) - 1                   | Cc     | box-C-class-c-prob     | tensor-continue, class-c-plane-end         |
|NUM_CANDIDATES × (NUM_CLASSES + 4)                       | M0     | box-0-mask-coeff-0     | tensor-continue, mask-coeff-0-plane-start  |
|...                                                      | ...    | ...                    | ...                                        |
|NUM_CANDIDATES × (NUM_CLASSES + 5) - 1                   | M0     | box-C-mask-coeff-0     | tensor-continue, mask-coeff-0-plane-end    |
|NUM_CANDIDATES × (NUM_CLASSES + 5)                       | M1     | box-0-mask-coeff-1     | tensor-continue, mask-coeff-1-plane-start  |
|...                                                      | ...    | ...                    | ...                                        |
|NUM_CANDIDATES × (NUM_CLASSES + 6) - 1                   | M1     | box-C-mask-coeff-1     | tensor-continue, mask-coeff-1-plane-end    |
|...                                                      | ...    | ...                    | ...                                        |
|NUM_CANDIDATES × (NUM_CLASSES + NUM_MASKS + 3)           | Mm     | box-0-mask-coeff-m     | tensor-continue, mask-coeff-m-plane-start  |
|...                                                      | ...    | ...                    | ...                                        |
|NUM_CANDIDATES × (NUM_CLASSES + NUM_MASKS + 4) - 1       | Mm     | box-C-mask-coeff-m     | tensor-end, mask-coeff-m-plane-end         |
| -                                                       | -      | -                      | -                                          |


# External References

* [YOLOv8 Paper](https://arxiv.org/abs/2305.09972)
* [Ultralytics YOLOv8 Documentation](https://docs.ultralytics.com/models/yolov8/)

# Models

* [YOLOv8s-seg ONNX trained on COCO](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/yolov8s-seg.onnx)
