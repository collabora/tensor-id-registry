# Classification

- tensor-group: no  
- layer-type: output  
- use-case: object-detection, pose-estimation, segmentation
- part-of-tensor-groups:  
    - [yolo-v8-pose-out](/tensor-groups/yolo-v8-pose-out.md)

# Description

Objectness confidence for each of the 8400 raw grid-cell predictions in the YOLOv8-Pose ONNX output (at 640 × 640 input).

## Scores Tensor

- **tensor-shape**: 1 × 8400  
- **tensor-datatype**: float32

### Known Aliases

* `detection_scores:0`

### Encoding

Each value is in range [0, 1], and indicates how confident the model is that the corresponding bounding box contains a person.


Memory layout of tensor data:

| Index  | Symbol           | Value                             | Comment               |
|--------|------------------|-----------------------------------|-----------------------|
| –      | –                | –                                 | –                     |
| 0      | tensor-start     | det-1-objet-class-score           | tensor-data, start    |
| 1      | tensor-continue  | det-2-objet-class-score           | tensor-data, continue |
| …      | …                | …                                 | tensor-data, continue |
| 8399   | tensor-end       | det-[8400-1]-objet-class-score    | tensor-data, end      |
| 8400   | –                | –                                 | –                     |


# External References

* [YOLOv8 Paper](https://arxiv.org/pdf/2305.09972)  
* [Ultralytics YOLOv8 Documentation](https://docs.ultralytics.com/)

# Models

* [ONNX model trained on COCO dataset](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/yolov8s-pose.onnx)

