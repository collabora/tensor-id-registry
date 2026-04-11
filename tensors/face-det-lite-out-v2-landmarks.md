# Classification

- tensor-group: no
- layer-type: output
- use-case: face-detection
- part-of-tensor-groups:
    - [face-det-lite-out-v2](/tensor-groups/face-det-lite-out-v2.md)

# Description

Per-cell facial landmark offset map produced by the Qualcomm Lightweight Face Detector (FaceDetLite) ONNX export.

## Landmarks Tensor

- tensor-shape: 1 × 10 × FM_H × FM_W
- tensor-datatype: float32
- tensor-id: face-det-lite-out-v2-landmarks

Where:
- `FM_H` = input height / 8 (e.g. 60 for a 480-pixel-tall input)
- `FM_W` = input width / 8 (e.g. 80 for a 640-pixel-wide input)
- The tensor is in channel-first layout (N × C × H × W), matching the
  native ONNX export of FaceDetLite.

### Known Aliases

- `landmark` (ONNX export output name, when present)

### Encoding

Each spatial cell `(cy, cx)` holds 10 values encoding the displacement from the cell position
to each of 5 facial landmarks, measured in units of stride (8 pixels per unit).

The 10 channels are laid out as:
- channels 0–4: `dx_1` … `dx_5` – x-displacement from cx for landmarks 1–5
- channels 5–9: `dy_1` … `dy_5` – y-displacement from cy for landmarks 1–5

For a face center detected at heatmap cell `(cx, cy)`, decode landmark `k` (k = 1…5) as:

```
landmark_k_x = (dx_k + cx) × stride
landmark_k_y = (dy_k + cy) × stride
```

where `stride = 8` and `dx_k = landmark[0, k-1, cy, cx]`, `dy_k = landmark[0, 4+k, cy, cx]`.

Memory layout of tensor data (shape `1 × 10 × FM_H × FM_W`, row-major channel-first):

|Index                                 |Value                                  |
|---                                   |---                                    |
|0                                     |cell-(0,0)-dx-landmark-1               |
|1                                     |cell-(0,1)-dx-landmark-1               |
|...                                   |...                                    |
|FM_W - 1                              |cell-(0,FM_W-1)-dx-landmark-1          |
|FM_W                                  |cell-(1,0)-dx-landmark-1               |
|...                                   |...                                    |
|FM_H × FM_W - 1                       |cell-(FM_H-1,FM_W-1)-dx-landmark-1     |
|FM_H × FM_W                           |cell-(0,0)-dx-landmark-2               |
|...                                   |...                                    |
|5 × FM_H × FM_W                       |cell-(0,0)-dy-landmark-1               |
|...                                   |...                                    |
|10 × FM_H × FM_W - 1                  |cell-(FM_H-1,FM_W-1)-dy-landmark-5     |

# External References

* [FaceDetLite source (Qualcomm AI Hub Models)](https://github.com/quic/ai-hub-models/tree/main/src/qai_hub_models/models/face_det_lite)
* [Objects as Points – CenterNet paper (Zhou et al., 2019)](https://arxiv.org/abs/1904.07850)

# Models

* ONNX-exported variant (refer to project ONNX model releases / artifacts for exact file)

# Tensor Decoders

|Framework | Links |
|---       |---    |
