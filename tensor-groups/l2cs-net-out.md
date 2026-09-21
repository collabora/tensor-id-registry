# L2CS-Net Gaze (Yaw + Pitch)

- tensor-group: yes
- layer-type: output
- use-case: gaze-estimation

## Description

L2CS-Net predicts 3D gaze direction from a face image as two
tensors, [yaw] and [pitch]. These are two separate instances of the
exact same [Hopenet fine-grained rotation-bin encoding][yaw]
(identical shape, datatype and decode formula — only the semantic
label and producing FC head differ; see [pitch] for how it documents
its equivalence to [yaw]). Hopenet originally produced 3 such
instances for head pose estimation (yaw/pitch/roll, one per FC head
sharing a common backbone); L2CS-Net reuses the same tensor for its
yaw and pitch outputs and drops the roll instance, since gaze
direction does not need it. Because both tensors share one encoding,
a decoder only needs a single implementation of the bin-decoding
logic, called once per output tensor.

This 2-tensor output contract (yaw-bins + pitch-bins, each decoded
independently via softmax-expectation and then combined into a gaze
vector) was introduced by L2CS-Net, built directly on top of
Hopenet's ResNet backbone and classification heads. It has since been
reused unmodified by other gaze-estimation projects, including
MobileGaze (yakhyo/gaze-estimation), which offers ResNet-18/34/50,
MobileNetV2 and MobileOne backbones, all exporting the identical
[yaw]/[pitch] tensor pair (trained on the Gaze360 dataset, or
optionally MPIIGaze with a different bin configuration).

This group describes the canonical **pre-softmax** contract. The
official L2CS-Net model returns the two linear-head values directly,
and its pipeline applies `Softmax` after inference. MobileGaze follows
the same boundary: its models return linear-head values, its ONNX
export exports those values, and its ONNX decoder applies softmax.
Exports that include softmax in the graph instead use
[l2cs-net-softmaxed-out].

Tensor identity should be carried explicitly rather than inferred from
position for the official implementation: `model.py` returns variables
named `(yaw, pitch)`, while `pipeline.py` unpacks the same pair as
`(pitch, yaw)`. MobileGaze's ONNX export resolves this ambiguity with
named outputs in `(yaw, pitch)` order.

L2CS-Net Output Tensors:
|Name|Shape|Description|
|---|---|---|
|[yaw]|1 x NUM_BINS|Binned classification logits for the horizontal (yaw) gaze angle|
|[pitch]|1 x NUM_BINS|Binned classification logits for the vertical (pitch) gaze angle|

## Tensor Decoding Logic

```
# NUM_BINS, BIN_WIDTH (degrees) and ANGLE_OFFSET (degrees) are dataset dependent.
# Gaze360 (used by L2CS-Net and MobileGaze): NUM_BINS=90, BIN_WIDTH=4, ANGLE_OFFSET=180
# MPIIGaze (used by MobileGaze):             NUM_BINS=28, BIN_WIDTH=3, ANGLE_OFFSET=42

yaw_probs   = softmax(yaw)
pitch_probs = softmax(pitch)

idx = [0, 1, ..., NUM_BINS - 1]

yaw_degrees   = sum(yaw_probs[i]   * idx[i] for i in 0:NUM_BINS-1) * BIN_WIDTH - ANGLE_OFFSET
pitch_degrees = sum(pitch_probs[i] * idx[i] for i in 0:NUM_BINS-1) * BIN_WIDTH - ANGLE_OFFSET

yaw_radians   = yaw_degrees   * pi / 180
pitch_radians = pitch_degrees * pi / 180

# Gaze direction unit vector (camera/eye coordinate system)
gaze_x = -cos(pitch_radians) * sin(yaw_radians)
gaze_y = -sin(pitch_radians)
gaze_z = -cos(pitch_radians) * cos(yaw_radians)
```

## External References

* [L2CS-Net: Fine-Grained Gaze Estimation in Unconstrained Environments Paper](https://arxiv.org/abs/2203.03339)
* [Fine-Grained Head Pose Estimation Without Keypoints (Hopenet) Paper](https://arxiv.org/abs/1710.00925) — original source of the binned-angle classification encoding reused by L2CS-Net
* [Gaze360: Physically Unconstrained Gaze Estimation in the Wild Paper](https://arxiv.org/abs/1905.02718) — dataset used to train the Gaze360-bins variant of this tensor-group
* [Official L2CS-Net model returns FC-head values without softmax](https://github.com/Ahmednull/L2CS-Net/blob/a4d8f7fa5436a2b2b9f088471623b552a85811bd/l2cs/model.py#L67-L70)
* [Official L2CS-Net pipeline applies softmax after model inference and decodes with bin indices](https://github.com/Ahmednull/L2CS-Net/blob/a4d8f7fa5436a2b2b9f088471623b552a85811bd/l2cs/pipeline.py#L121-L131)
* [MobileGaze exports named yaw then pitch outputs](https://github.com/yakhyo/gaze-estimation/blob/27efe989fc45e1e2f28e1ed5f20cedfeb75a272c/onnx_export.py#L73-L95)
* [MobileGaze ONNX decoder applies softmax outside the graph](https://github.com/yakhyo/gaze-estimation/blob/27efe989fc45e1e2f28e1ed5f20cedfeb75a272c/onnx_inference.py#L72-L94)

## Models

* [L2CS-Net official PyTorch implementation](https://github.com/Ahmednull/L2CS-Net)
* [MobileGaze (yakhyo/gaze-estimation) — MobileNetV2 and other backbones](https://github.com/yakhyo/gaze-estimation)
* [MobileGaze ONNX export script](https://github.com/yakhyo/gaze-estimation/blob/main/onnx_export.py)
* [py-feat/l2cs Hugging Face port](https://huggingface.co/py-feat/l2cs)

## Tensor Decoders
|Framework|Links|
|---|---|
|pytorch|[L2CS-Net Pipeline.predict_gaze](https://github.com/Ahmednull/L2CS-Net/blob/main/l2cs/pipeline.py)|
|onnxruntime|[MobileGaze GazeEstimationONNX.decode](https://github.com/yakhyo/gaze-estimation/blob/main/onnx_inference.py)|

[yaw]: /tensors/hopenet-out-yaw.md
[pitch]: /tensors/hopenet-out-pitch.md
[l2cs-net-softmaxed-out]: /tensor-groups/l2cs-net-softmaxed-out.md
