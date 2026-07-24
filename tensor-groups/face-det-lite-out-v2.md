# Classification

- tensor-group: yes
- layer-type: output
- use-case: face-detection

# Description

Output tensors of the Qualcomm Lightweight Face Detector (FaceDetLite) ONNX export variant. This file mirrors
`/tensor-groups/face-det-lite-out.md` but points to the ONNX-exported model variant and separate v2 tensor files.

The ONNX-exported model preserves the original interpretation: three spatial output maps at stride 8 that jointly
encode face centers, bounding boxes and facial landmarks. All three output tensors share the same spatial resolution
`FM_H × FM_W` where `FM_H = input_height / 8` and `FM_W = input_width / 8`.

FaceDetLite (ONNX) Output Tensors:

| Name               | Shape                      | Description                                   |
|---                 |---                         |---                                            |
| [heatmap-v2]       | 1 × 1 × FM_H × FM_W        | Face-center confidence logits (ONNX export)   |
| [boxes-v2]         | 1 × 4 × FM_H × FM_W        | Per-cell bounding box edge offsets            |
| [landmarks-v2]     | 1 × 10 × FM_H × FM_W       | Per-cell facial landmark offsets (5 points)   |

All v2 tensors use channel-first (NCHW) layout matching the native PyTorch/ONNX
export of FaceDetLite. (The v1 TFLite variant uses channel-last NHWC instead.)

# Tensor Decoding Logic

Decoding and post-processing are identical to the original FaceDetLite variant: activate heatmap with sigmoid,
perform local-maximum suppression, decode per-cell box and landmark offsets (stride = 8), then apply NMS on
final detections. See `/tensor-groups/face-det-lite-out.md` for full pseudocode.

# External References

* [FaceDetLite source (Qualcomm AI Hub Models)](https://github.com/quic/ai-hub-models/tree/main/src/qai_hub_models/models/face_det_lite)
* [Objects as Points – CenterNet paper (Zhou et al., 2019)](https://arxiv.org/abs/1904.07850)
* [Searching for MobileNetV3 – backbone paper (Howard et al., 2019)](https://arxiv.org/abs/1905.02244)

# Models

* ONNX-exported variant (refer to project ONNX model releases / artifacts for exact file)
* [Qualcomm Lightweight-Face-Detection on Hugging Face](https://huggingface.co/qualcomm/Lightweight-Face-Detection)

# Tensor Decoders

|Framework | Links |
|---       |---    |

[heatmap-v2]:   /tensors/face-det-lite-out-v2-heatmap.md
[boxes-v2]:     /tensors/face-det-lite-out-v2-boxes.md
[landmarks-v2]: /tensors/face-det-lite-out-v2-landmarks.md
