# Classification

- tensor-group: no
- layer-type: output
- use-case: object-detection, pose-estimation, segmentation
- part-of-tensor-groups:
    - [yolo-v8-pose-out](/tensor-groups/yolo-v8-pose-out.md)

# Description

Bounding‐box parameters for each of the 8400 raw grid‐cell predictions in the YOLOv8‐Pose ONNX output (at 640 × 640 input).

## Boxes Tensor

- tensor-shape: 1 × 8400 × 4  
- tensor-datatype: float32

### Known Aliases
* detection_boxes:0

### Encoding

Center-based format, normalized to [0,1]:

Scheme: (X: box x center, Y: box y center, W: box width, H: box height)

|box-1 | box-1 | box-1 | box-1 | box-2 | box-2 | box-2 | box-2 | ... | box-8400 | box-8400 | box-8400 | box-8400 |
|---   |---    |---    |---    |---    |---    |---    |---    |---  |---                |---                |---                |---                |
| X | Y | W | H | X | Y | W | H |...| X | Y | W | H |

Memory layout of tensor data:

|Index                  | Symbol            |Value              | Comment                             |
|---                    |---                |---                |---                                  |
| -                     | -                 | -                 | -                                   |
|0                      |                   |x-center            |tensor-start, box-0-tensor-data      |
|1                      |                   |y-center            |tensor-continue, box-0-tensor-data   |
|2                      |                   |width              |tensor-continue, box-0-tensor-data   |
|3                      |                   |height             |tensor-continue, box-0-tensor-data   |
|4                      |                   |x-center            |tensor-continue, box-1-tensor-data   |
|5                      |                   |y-center            |tensor-continue, box-1-tensor-data   |
|6                      |                   |width              |tensor-continue, box-1-tensor-data   |
|7                      |                   |height             |tensor-continue, box-1-tensor-data   |
|...                    | ...               | ...               | ...                                 |
|([8400] − 1) x 4      |                   |x-center            |tensor-continue, box-8400-tensor-data |
|([8400] − 1) x 4 + 1  |                   |y-center            |tensor-continue, box-8400-tensor-data |
|([8400] − 1) x 4 + 2  |                   |width              |tensor-continue, box-8400-tensor-data |
|([8400] − 1) x 4 + 3  |                   |height             |tensor-continue, box-8400-tensor-data |
| -                     | -                 | -                 | -                                   |

# External References

* [YOLOv8 Paper](https://arxiv.org/pdf/2305.09972)
* [Ultralytics YOLOv8 Documentation](https://docs.ultralytics.com/)  

# Models

* [ONNX model trained on COCO dataset](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/yolov8s-pose.onnx)
