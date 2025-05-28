# Classification
- tensor-group: yes
- layer-type: output
- use-case: pose-estimation

# Description

Variant of YOLOv8-Pose outputs. This tensor group contains three tensors that 
together describe the predictions of yolov8-pose. The “8400” comes from the total 
number of grid-cell predictions at strides [8,16,32] for a 640 × 640 input:
```python
(640/8)**2 + (640/16)**2 + (640/32)**2  # 80² + 40² + 20² = 8400
```

YOLOv8-Pose Output Tensors:

| Name          |shape               |
|---            |---                 |
| [boxes]       |1 x 8400 x 4        |
| [scores]      |1 x 8400            |
| [keypoints]   |1 × 8400 × 17 × 3   |

# Tensor Decoding Logic

```
raw = output0                                   # (1, 56, 8400)
outputs = np.transpose(np.squeeze(raw[0]))      # (8400, 56)
detections = []

Foreach i in outputs.shape[0]:
    row = outputs[i]
    X, Y, W, H = row[0], row[1], row[2], row[3]
    S          = row[4]

    # collect 17 keypoints (x, y, conf)
    kps = []
    Foreach k in 17:
        base = 5 + 3*k
        KX = row[base]
        KY = row[base+1]
        KC = row[base+2]
        kps.append([KX, KY, KC])
    # class-id is 0 since people are detected
    detections[i] = [X, Y, W, H, 0, S, kps]
```

# External References

* [Yolov8 Paper](https://arxiv.org/pdf/2305.09972)

# Models

* [ONNX model trained on COCO dataset](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/yolov8s-pose.onnx)



[boxes]: /tensors/yolo-v8-pose-out-boxes.md
[scores]: /tensors/yolo-v8-pose-out-scores.md
[key-points]: /tensors/yolo-v8-pose-out-key-points.md
