# Classification
- tensor-group: yes
- layer-type: output
- use-case: pose-estimation

# Description

This tensor encodes, for each of the 8400 grid cells, its bounding-box (X, Y, W, H),  
the person-confidence score S, and all 17 keypoints (x, y, confidence). The `8400` comes 
from the total number of grid-cell predictions at strides [8,16,32] for a 640 × 640 input:
```python
(640/8)**(640/8) + (640/16)**(640/16) + (640/32)**(640/32)  # 80² + 40² + 20² = 8400
```
> NOTE: The [onnx model](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/yolov8s-pose.onnx) used to infer the shapes was exported with a static input shape (640, 640, 3).

YOLOv8-Pose Output Tensor:

| Name          | shape               |
|---            | ---                 |
| [output]      | 1 x 56 x 8400       |


# Tensor Decoding Logic

```
raw = output0                                           # (1, 56, 8400)
outputs = np.transpose(np.squeeze(raw[0]))              # (8400, 56)
detections = []

Foreach i in outputs.shape[0]:
    row = outputs[i]
    X, Y, W, H = row[0], row[1], row[2], row[3]         # bbox coordinates, width, height     
    S          = row[4]                                 # person-confidence score

    # collect 17 keypoints (x, y, conf)
    kps = []
    Foreach k in 17:
        base = 5 + 3*k
        KX = row[base]                                  # Keypoint x coord
        KY = row[base+1]                                # Keypoint y coord
        KC = row[base+2]                                # Keypoint confidence score
        kps.append([KX, KY, KC])
    # class-id is 0 since people are detected
    detections[i] = [X, Y, W, H, 0, S, kps]
```

### Encoding

Center-based format, normalized to [0,1]:

Scheme: [X, Y, W, H, S, K1X, K1Y, K1C, ..., K17X, K17Y, K17C]

- X, Y = box center
- W, H = box width/height
- S = person-confidence
- KkX, KkY, KkC = keypoint k (x, y, confidence), k = 1...17

Memory layout of output tensor data (shape `1×56×8400`):

Row-major (C-order): first all 8400 X's, then 8400 Y's, then W's, H's, S's, K1X's, ..., up to K17C's.

| Index Range                     | Symbol | Value                     | Comment                                      |
|---------------------------------|--------|---------------------------|----------------------------------------------|
| 0           ... [8400 x 1] - 1  | `X`    | x-center                  | tensor-start, box-0-8399-tensor-data         |
| [8400 x 1]  ... [8400 x 2] - 1  | `Y`    | y-center                  | tensor-continue, box-0-8399-tensor-data      |
| [8400 x 2]  ... [8400 x 3] - 1  | `W`    | width                     | tensor-continue, box-0-8399-tensor-data      |
| [8400 x 3]  ... [8400 x 4] - 1  | `H`    | height                    | tensor-continue, box-0-8399-tensor-data      |
| [8400 x 4]  ... [8400 x 5] - 1  | `S`    | confidence score          | tensor-continue, box-0-8399-tensor-data      |
| [8400 x 5]  ... [8400 x 6] - 1  | `K1X`  | keypoint 1 x-coord        | tensor-continue, box-0-8399-tensor-data      |
| [8400 x 6]  ... [8400 x 7] - 1  | `K1Y`  | keypoint 1 y-coord        | tensor-continue, box-0-8399-tensor-data      |
| [8400 x 7]  ... [8400 x 8] - 1  | `K1C`  | keypoint 1 confidence     | tensor-continue, box-0-8399-tensor-data      |
| ...                             | ...    | ...                       | ...                                          |
| [8400 x 53] ... [8400 x 54] - 1 | `K17X` | keypoint 17 x-coord       | tensor-continue, box-0-8399-tensor-data      |
| [8400 x 54] ... [8400 x 55] - 1 | `K17Y` | keypoint 17 y-coord       | tensor-continue, box-0-8399-tensor-data      |
| [8400 x 55] ... [8400 x 56] - 1 | `K17C` | keypoint 17 confidence    | tensor-continue, box-0-8399-tensor-data      |



---

# External References

* [Yolov8 Paper](https://arxiv.org/pdf/2305.09972)

# Models

* [ONNX model trained on COCO dataset](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/yolov8s-pose.onnx)
