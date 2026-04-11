# Classification

- tensor-group: yes
- layer-type: output
- use-case: face-detection

# Description

Output tensors of the Qualcomm Lightweight Face Detector (FaceDetLite) as observed in the TFLite export.
This is an anchor-free, CenterNet-style face detector developed by Qualcomm. The TFLite-exported
model outputs three spatial feature maps at stride 8 that jointly encode face center locations,
bounding boxes, and facial landmarks.

The model backbone is a lightweight MobileNetV3-small variant (Mbv3SmallFast) accepting a
single-channel (grayscale) input. All three output tensors share the same spatial resolution
`FM_H × FM_W` where `FM_H = input_height / 8` and `FM_W = input_width / 8`.

Note: For the ONNX-exported variant see the v2 registry entries at `/tensor-groups/face-det-lite-out-v2.md`.

FaceDetLite Output Tensors:

| Name           | Shape                      | Description                                   |
|---             |---                         |---                                            |
| [heatmap]      | 1 × FM_H × FM_W × 1        | Face-center confidence logits                 |
| [boxes]        | 1 × FM_H × FM_W × 4        | Per-cell bounding box edge offsets            |
| [landmarks]    | 1 × FM_H × FM_W × 10       | Per-cell facial landmark offsets (5 points)   |

# Tensor Decoding Logic

```
# 1. Activate heatmap and find local maxima (heatmap NMS)
hm = sigmoid(heatmap[0, :, :, 0])                 # FM_H × FM_W
hm_pool = max_pool2d(hm, kernel=3, stride=1, pad=1)
peaks = hm * (hm == hm_pool)                       # zero out non-maxima

# 2. Collect top-k candidate face centers above threshold
scores, indices = topk(peaks.flatten(), k=2000)
Foreach (score, flat_idx) where score >= threshold:
    cy = flat_idx / FM_W
    cx = flat_idx % FM_W

    # 3. Decode bounding box from per-cell offsets
    offset_left, offset_top, offset_right, offset_bottom = boxes[0, cy, cx, :]
    face_left   = (cx - offset_left)   * stride    # stride = 8
    face_top    = (cy - offset_top)    * stride
    face_right  = (cx + offset_right)  * stride
    face_bottom = (cy + offset_bottom) * stride

    # 4. Decode facial landmarks from per-cell offsets
    Foreach k in 0..4:
        landmark_k_x = (landmarks[0, cy, cx, k]     + cx) * stride
        landmark_k_y = (landmarks[0, cy, cx, 5 + k] + cy) * stride

# 5. (Optional) Apply IoU-based NMS across candidates
detections = nms(detections, iou_threshold)
```

# External References

* [FaceDetLite source (Qualcomm AI Hub Models)](https://github.com/quic/ai-hub-models/tree/main/src/qai_hub_models/models/face_det_lite)
* [Objects as Points – CenterNet paper (Zhou et al., 2019)](https://arxiv.org/abs/1904.07850)
* [Searching for MobileNetV3 – backbone paper (Howard et al., 2019)](https://arxiv.org/abs/1905.02244)

# Models

* [Qualcomm Lightweight-Face-Detection on Hugging Face](https://huggingface.co/qualcomm/Lightweight-Face-Detection)

# Tensor Decoders

|Framework | Links |
|---       |---    |


[heatmap]:   /tensors/face-det-lite-out-heatmap.md
[boxes]:     /tensors/face-det-lite-out-boxes.md
[landmarks]: /tensors/face-det-lite-out-landmarks.md
