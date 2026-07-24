# Classification

- tensor-group: no
- layer-type: output
- use-case: face-detection
- part-of-tensor-groups:
    - [face-det-lite-out](/tensor-groups/face-det-lite-out.md)

# Description

Per-cell bounding box offset map produced by the Qualcomm Lightweight Face Detector (FaceDetLite) TFLite export.

## Boxes Tensor

- tensor-shape: 1 × FM_H × FM_W × 4
- tensor-datatype: float32
- tensor-id: face-det-lite-out-boxes

Where:
- `FM_H` = input height / 8 (e.g. 60 for a 480-pixel-tall input)
- `FM_W` = input width / 8 (e.g. 80 for a 640-pixel-wide input)
- The tensor is in channel-last layout (N × H × W × C)

### Known Aliases

- `bbox` (Qualcomm AI Hub model output name)
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

Memory layout of tensor data (shape `1 × FM_H × FM_W × 4`, row-major channel-last):

|Index                          |Value                                  |
|---                            |---                                    |
|0                              |cell-(0,0)-offset-left                 |
|1                              |cell-(0,0)-offset-top                  |
|2                              |cell-(0,0)-offset-right                |
|3                              |cell-(0,0)-offset-bottom               |
|4                              |cell-(0,1)-offset-left                 |
|5                              |cell-(0,1)-offset-top                  |
|6                              |cell-(0,1)-offset-right                |
|7                              |cell-(0,1)-offset-bottom               |
|...                            |...                                    |
|FM_W × 4 - 4                   |cell-(0,FM_W-1)-offset-left            |
|FM_W × 4 - 3                   |cell-(0,FM_W-1)-offset-top             |
|FM_W × 4 - 2                   |cell-(0,FM_W-1)-offset-right           |
|FM_W × 4 - 1                   |cell-(0,FM_W-1)-offset-bottom          |
|FM_W × 4                       |cell-(1,0)-offset-left                 |
|...                            |...                                    |
|(FM_H × FM_W - 1) × 4         |cell-(FM_H-1,FM_W-1)-offset-left       |
|(FM_H × FM_W - 1) × 4 + 1     |cell-(FM_H-1,FM_W-1)-offset-top        |
|(FM_H × FM_W - 1) × 4 + 2     |cell-(FM_H-1,FM_W-1)-offset-right      |
|(FM_H × FM_W - 1) × 4 + 3     |cell-(FM_H-1,FM_W-1)-offset-bottom     |

# External References

* [FaceDetLite source (Qualcomm AI Hub Models)](https://github.com/quic/ai-hub-models/tree/main/src/qai_hub_models/models/face_det_lite)
* [Objects as Points – CenterNet paper (Zhou et al., 2019)](https://arxiv.org/abs/1904.07850)

# Models

* [Qualcomm Lightweight-Face-Detection on Hugging Face](https://huggingface.co/qualcomm/Lightweight-Face-Detection)
* See ONNX variant: /tensor-groups/face-det-lite-out-v2.md

# Tensor Decoders

|Framework | Links |
|---       |---    |
