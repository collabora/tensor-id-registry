# Classification

- tensor-group: no
- layer-type: output
- use-case: instance-segmentation
- part-of-tensor-groups:
    - [yolo-v8-segmentation-out](/tensor-groups/yolo-v8-segmentation-out.md)

# Description

Prototype mask patterns used for generating instance segmentation masks in YOLOv8. These
learned basis functions are combined with mask coefficients from the [detection predictions tensor](/tensors/yolo-v8-segmentation-out-detections.md)
to create final segmentation masks for each detected object.

## Prototype Masks Tensor

- tensor-shape: BATCH_SIZE x NUM_MASKS x PROTO_H x PROTO_W
- tensor-datatype: float32
- memory-layout: column-major order

Where:
 * BATCH_SIZE is the size of the batch
 * NUM_MASKS is the number of prototype masks (must match NUM_MASKS in the [detection predictions tensor](/tensors/yolo-v8-segmentation-out-detections.md))
 * PROTO_H and PROTO_W are the spatial dimensions of each prototype mask, determined by the model architecture (typically input_height/4 × input_width/4)

### Known Aliases
* output1

### Encoding

The tensor contains NUM_MASKS prototype mask patterns, each of size PROTO_H×PROTO_W pixels. These serve as basis functions for mask generation.

Scheme: (P: prototype pattern value at spatial location)

|Proto-0         | Proto-1         | Proto-2         | ... | Proto-m             |
|---             |---              |---              |---  |---                  |
|PROTO_H×PROTO_W |PROTO_H×PROTO_W  |PROTO_H×PROTO_W  |...  |PROTO_H×PROTO_W      |

Memory layout of tensor data:

|Index                                      | Symbol | Value                        | Comment                                    |
|---                                        |---     |---                           |---                                         |
| -                                         | -      | -                            | -                                          |
|0                                          | P0     | proto-0-pixel-(0,0)          | tensor-start, proto-0-plane-start          |
|1                                          | P0     | proto-0-pixel-(0,1)          | tensor-continue, proto-0-plane-continue    |
|...                                        | ...    | ...                          | ...                                        |
|(PROTO_H × PROTO_W) - 1                    | P0     | proto-0-pixel-(PROTO_H-1,PROTO_W-1)    | tensor-continue, proto-0-plane-end         |
|PROTO_H × PROTO_W                          | P1     | proto-1-pixel-(0,0)          | tensor-continue, proto-1-plane-start       |
|...                                        | ...    | ...                          | ...                                        |
|(2 × PROTO_H × PROTO_W) - 1               | P1     | proto-1-pixel-(PROTO_H-1,PROTO_W-1)    | tensor-continue, proto-1-plane-end         |
|2 × PROTO_H × PROTO_W                     | P2     | proto-2-pixel-(0,0)          | tensor-continue, proto-2-plane-start       |
|...                                        | ...    | ...                          | ...                                        |
|(3 × PROTO_H × PROTO_W) - 1               | P2     | proto-2-pixel-(PROTO_H-1,PROTO_W-1)    | tensor-continue, proto-2-plane-end         |
|...                                        | ...    | ...                          | ...                                        |
|(NUM_MASKS - 1) × PROTO_H × PROTO_W       | Pm     | proto-m-pixel-(0,0)          | tensor-continue, proto-m-plane-start       |
|(NUM_MASKS - 1) × PROTO_H × PROTO_W + 1   | Pm     | proto-m-pixel-(0,1)          | tensor-continue, proto-m-plane-continue    |
|...                                        | ...    | ...                          | ...                                        |
|(NUM_MASKS × PROTO_H × PROTO_W) - 1       | Pm     | proto-m-pixel-(PROTO_H-1,PROTO_W-1)    | tensor-end, proto-m-plane-end              |
| -                                         | -      | -                            | -                                          |


# External References

* [YOLOv8 Paper](https://arxiv.org/abs/2305.09972)
* [Ultralytics YOLOv8 Documentation](https://docs.ultralytics.com/models/yolov8/)

# Models

* [YOLOv8s-seg ONNX trained on COCO](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/yolov8s-seg.onnx)

