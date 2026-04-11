# Classification

- tensor-group: no
- layer-type: output
- use-case: face-detection
- part-of-tensor-groups:
    - [face-det-lite-out-v2](/tensor-groups/face-det-lite-out-v2.md)

# Description

Face-center confidence heatmap produced by the Qualcomm Lightweight Face Detector (FaceDetLite) ONNX export.

## Heatmap Tensor

- tensor-shape: 1 × 1 × FM_H × FM_W
- tensor-datatype: float32
- tensor-id: face-det-lite-out-v2-heatmap

Where:
- `FM_H` = input height / 8 (e.g. 60 for a 480-pixel-tall input)
- `FM_W` = input width / 8 (e.g. 80 for a 640-pixel-wide input)
- The tensor is in channel-first layout (N × C × H × W), matching the
  native ONNX export of FaceDetLite. Since `C = 1`, the in-memory layout
  is identical to the channel-last form.

### Known Aliases

- `heatmap` (ONNX export output name, when present)
- `center`  (internal model variable name)

### Encoding

Each spatial cell `(cy, cx)` holds a single raw logit indicating the probability that a face
center lies at the corresponding location in the input image (coordinates `(cx × 8, cy × 8)`).
The logit must be passed through a sigmoid function to obtain the probability in `[0, 1]`.

During decoding, local maxima in the sigmoid-activated heatmap are selected as face center
candidates (via max-pool based non-maximum suppression), and only candidates above a score
threshold are retained.

Memory layout of tensor data (shape `1 × 1 × FM_H × FM_W`, row-major):

|Index                      |Value                              |
|---                        |---                                |
|0                          |cell-(0,0)-logit                   |
|1                          |cell-(0,1)-logit                   |
|...                        |...                                |
|FM_W - 1                   |cell-(0,FM_W-1)-logit              |
|FM_W                       |cell-(1,0)-logit                   |
|...                        |...                                |
|FM_H × FM_W - 1            |cell-(FM_H-1,FM_W-1)-logit         |

# External References

* [FaceDetLite source (Qualcomm AI Hub Models)](https://github.com/quic/ai-hub-models/tree/main/src/qai_hub_models/models/face_det_lite)
* [Objects as Points – CenterNet paper (Zhou et al., 2019)](https://arxiv.org/abs/1904.07850)

# Models

* ONNX-exported variant (refer to project ONNX model releases / artifacts for exact file)

# Tensor Decoders

|Framework | Links |
|---       |---    |
