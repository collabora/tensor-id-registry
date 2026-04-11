# Classification

- tensor-group: no
- layer-type: output
- use-case: face-detection
- part-of-tensor-groups:
    - [face-det-lite-out](/tensor-groups/face-det-lite-out.md)

# Description

Per-cell facial landmark offset map produced by the Qualcomm Lightweight Face Detector (FaceDetLite) TFLite export.

## Landmarks Tensor

- tensor-shape: 1 × FM_H × FM_W × 10
- tensor-datatype: float32
- tensor-id: face-det-lite-out-landmarks

Where:
- `FM_H` = input height / 8 (e.g. 60 for a 480-pixel-tall input)
- `FM_W` = input width / 8 (e.g. 80 for a 640-pixel-wide input)
- The tensor is in channel-last layout (N × H × W × C)

### Known Aliases

- `landmark` (Qualcomm AI Hub model output name)

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

where `stride = 8` and `dx_k = landmark[0, cy, cx, k-1]`, `dy_k = landmark[0, cy, cx, 4+k]`.

Memory layout of tensor data (shape `1 × FM_H × FM_W × 10`, row-major channel-last):

|Index                           |Value                                  |
|---                             |---                                    |
|0                               |cell-(0,0)-dx-landmark-1               |
|1                               |cell-(0,0)-dx-landmark-2               |
|2                               |cell-(0,0)-dx-landmark-3               |
|3                               |cell-(0,0)-dx-landmark-4               |
|4                               |cell-(0,0)-dx-landmark-5               |
|5                               |cell-(0,0)-dy-landmark-1               |
|6                               |cell-(0,0)-dy-landmark-2               |
|7                               |cell-(0,0)-dy-landmark-3               |
|8                               |cell-(0,0)-dy-landmark-4               |
|9                               |cell-(0,0)-dy-landmark-5               |
|10                              |cell-(0,1)-dx-landmark-1               |
|...                             |...                                    |
|FM_W × 10 - 1                   |cell-(0,FM_W-1)-dy-landmark-5          |
|FM_W × 10                       |cell-(1,0)-dx-landmark-1               |
|...                             |...                                    |
|(FM_H × FM_W - 1) × 10          |cell-(FM_H-1,FM_W-1)-dx-landmark-1     |
|(FM_H × FM_W - 1) × 10 + 9      |cell-(FM_H-1,FM_W-1)-dy-landmark-5     |

# External References

* [FaceDetLite source (Qualcomm AI Hub Models)](https://github.com/quic/ai-hub-models/tree/main/src/qai_hub_models/models/face_det_lite)
* [Objects as Points – CenterNet paper (Zhou et al., 2019)](https://arxiv.org/abs/1904.07850)

# Models

* [Qualcomm Lightweight-Face-Detection on Hugging Face](https://huggingface.co/qualcomm/Lightweight-Face-Detection)
* See ONNX variant: /tensor-groups/face-det-lite-out-v2.md

# Tensor Decoders

|Framework | Links |
|---       |---    |
