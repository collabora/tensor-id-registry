# Classification

- tensor-group: no
- layer-type: output
- use-case: instance-segmentation
- part-of-tensor-groups:
    - [yolov8-segmentation-output](/tensor-groups/yolov8-segmentation-output.md)

# Description

Prototype mask patterns used for generating instance segmentation masks in YOLOv8. These
256 learned basis functions are combined with mask coefficients from the [detection predictions tensor](/tensors/yolov8-seg-detection-predictions.md) 
to create final segmentation masks for each detected object.

## Prototype Masks Tensor

- tensor-shape: 1 x 32 x 32 x 256
- tensor-datatype: float32
- memory-layout: column-major order

### Known Aliases
* output1

### Encoding

The tensor contains 256 prototype mask patterns, each of size 32×32 pixels. These serve as basis functions for mask generation.

Scheme: (P: prototype pattern value at spatial location)

|Proto-0 | Proto-1 | Proto-2 | ... | Proto-255 |
|---     |---      |---      |---  |---        |
|32×32   |32×32    |32×32    |...  |32×32      |

Memory layout of tensor data:

|Index                          | Symbol            | Value                    | Comment                                    |
|---                           |---                |---                       |---                                         |
| -                            | -                 | -                        | -                                          |
|0                             | P0                | proto-0-pixel-(0,0)      | tensor-start, proto-0-plane-start          |
|1                             | P0                | proto-0-pixel-(0,1)      | tensor-continue, proto-0-plane-continue    |
|...                           | ...               | ...                      | ...                                        |
|(32 × 32) - 1                 | P0                | proto-0-pixel-(31,31)    | tensor-continue, proto-0-plane-end         |
|32 × 32                       | P1                | proto-1-pixel-(0,0)      | tensor-continue, proto-1-plane-start       |
|...                           | ...               | ...                      | ...                                        |
|(2 × 32 × 32) - 1             | P1                | proto-1-pixel-(31,31)    | tensor-continue, proto-1-plane-end         |
|2 × (32 × 32)                 | P2                | proto-2-pixel-(0,0)      | tensor-continue, proto-2-plane-start       |
|...                           | ...               | ...                      | ...                                        |
|(3 × 32 × 32) - 1             | P2                | proto-2-pixel-(31,31)    | tensor-continue, proto-2-plane-end         |
|...                           | ...               | ...                      | ...                                        |
|255 × (32 × 32)               | P255              | proto-255-pixel-(0,0)    | tensor-continue, proto-255-plane-start     |
|255 × (32 × 32) + 1           | P255              | proto-255-pixel-(0,1)    | tensor-continue, proto-255-plane-continue  |
|...                           | ...               | ...                      | ...                                        |
|(256 × 32 × 32) - 1           | P255              | proto-255-pixel-(31,31)  | tensor-end, proto-255-plane-end            |
| -                            | -                 | -                        | -                                          |


# External References

* [YOLOv8 Paper](https://arxiv.org/abs/2305.09972)
* [Ultralytics YOLOv8 Documentation](https://docs.ultralytics.com/models/yolov8/)

# Models

* [YOLOv8s-seg ONNX trained on COCO](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/yolov8s-seg.onnx)

