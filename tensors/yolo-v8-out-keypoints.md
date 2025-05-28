<!-- File: tensors/yolov8-pose-out-keypoints.md -->
# Classification

- tensor-group: no  
- layer-type: output  
- use-case: object-detection, pose-estimation, segmentation  
- part-of-tensor-groups:  
    - [yolo-v8-pose-out](/tensor-groups/yolo-v8-pose-out.md)

# Description

17 body‐keypoints (x, y, confidence) for each of the 8 400 raw grid‐cell predictions in the YOLOv8‐Pose ONNX output (at 640 × 640 input).

## Keypoints Tensor

- **tensor-id**: `keypoints`  
- **tensor-shape**: 1 × 8400 × 17 × 3  
- **tensor-datatype**: float32  

### Known Aliases

* `detection_keypoints:0`

### Encoding

```
For each detection _i_ and keypoint _k_:
    keypoints[i,k] = (x_k, y_k, conf_k)
```
All values are normalized to [0,1].

Memory layout of tensor data:

| Index                    | Symbol           | Value          | Comment                                    |
|--------------------------|------------------|----------------|--------------------------------------------|
| –                        | –                | –              | –                                          |
| 0                        | tensor-start     | kp-0-x         | detection-0, keypoint-0, x                 |
| 1                        | tensor-continue  | kp-0-y         | detection-0, keypoint-0, y                 |
| 2                        | tensor-continue  | kp-0-conf      | detection-0, keypoint-0, confidence        |
| …                        | …                | …              | …                                          |
| 50                       | tensor-continue  | kp-16-conf     | detection-0, keypoint-16, confidence       |
| (8400 − 1) × 51          | tensor-continue  | kp-0-x         | detection-8399, keypoint-0, x              |
| (8400 − 1) × 51 + 1      | tensor-continue  | kp-0-y         | detection-8399, keypoint-0, y              |
| …                        | …                | …              | …                                          |
| (8400 − 1) × 51 + 50     | tensor-end       | kp-16-conf     | detection-8399, keypoint-16, confidence    |
| 8400 × 51                | –                | –              | –                                          |

# External References

* [YOLOv8 Paper](https://arxiv.org/pdf/2305.09972)
* [Ultralytics YOLOv8 Documentation](https://docs.ultralytics.com/)  

# Models

* [ONNX model trained on COCO dataset](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/yolov8s-pose.onnx)
