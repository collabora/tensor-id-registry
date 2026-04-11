# Classification

- tensor-group: no
- layer-type: output
- use-case: face-detection
- part-of-tensor-groups:
    - [face-det-lite-out-v2](/tensor-groups/face-det-lite-out-v2.md)

# Description

Per-cell bounding box offset map produced by the Qualcomm Lightweight Face Detector (FaceDetLite) ONNX export.

## Boxes Tensor

- tensor-shape: 1 × 4 × FM_H × FM_W
- tensor-datatype: float32
- tensor-id: face-det-lite-out-v2-boxes

Where:
- `FM_H` = input height / 8 (e.g. 60 for a 480-pixel-tall input)
- `FM_W` = input width / 8 (e.g. 80 for a 640-pixel-wide input)
- The tensor is in channel-first layout (N × C × H × W), matching the
  native ONNX export of FaceDetLite.

### Known Aliases

- `bbox` (ONNX export output name, when present)
- `box`  (internal model variable name)

### Encoding

Each spatial cell `(cy, cx)` holds four values encoding the displacement from the cell position
to the four edges of the face bounding box, measured in units of stride (8 pixels per unit):

- channel 0: `offset_left`   – displacement to the left edge   (subtract from cx)
- channel 1: `offset_top`    – displacement to the top edge    (subtract from cy)
- channel 2: `offset_right`  – displacement to the right edge  (add to cx)
- channel 3: `offset_bottom` – displacement to the bottom edge (add to cy)

For a face center detected at heatmap cell `(cx, cy)`, decode as:

```
face_left   = (cx - offset_left)   × stride
face_top    = (cy - offset_top)    × stride
face_right  = (cx + offset_right)  × stride
face_bottom = (cy + offset_bottom) × stride
```

where `stride = 8`.

Memory layout of tensor data (shape `1 × 4 × FM_H × FM_W`, row-major channel-first):

|Index                                 |Value                                  |
|---                                   |---                                    |
|0                                     |cell-(0,0)-offset-left                 |
|1                                     |cell-(0,1)-offset-left                 |
|...                                   |...                                    |
|FM_W - 1                              |cell-(0,FM_W-1)-offset-left            |
|FM_W                                  |cell-(1,0)-offset-left                 |
|...                                   |...                                    |
|FM_H × FM_W - 1                       |cell-(FM_H-1,FM_W-1)-offset-left       |
|FM_H × FM_W                           |cell-(0,0)-offset-top                  |
|...                                   |...                                    |
|2 × FM_H × FM_W                       |cell-(0,0)-offset-right                |
|...                                   |...                                    |
|3 × FM_H × FM_W                       |cell-(0,0)-offset-bottom               |
|...                                   |...                                    |
|4 × FM_H × FM_W - 1                   |cell-(FM_H-1,FM_W-1)-offset-bottom     |

# External References

* [FaceDetLite source (Qualcomm AI Hub Models)](https://github.com/quic/ai-hub-models/tree/main/src/qai_hub_models/models/face_det_lite)
* [Objects as Points – CenterNet paper (Zhou et al., 2019)](https://arxiv.org/abs/1904.07850)

# Models

* ONNX-exported variant (refer to project ONNX model releases / artifacts for exact file)

# Tensor Decoders

|Framework | Links |
|---       |---    |
