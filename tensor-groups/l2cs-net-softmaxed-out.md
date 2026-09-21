# L2CS-Net Gaze Probability Outputs (Yaw + Pitch)

- tensor-group: yes
- layer-type: output
- use-case: gaze-estimation

## Description

Post-softmax variant of [l2cs-net-out]. Both tensors are normalized
probability distributions over angle bins rather than raw
classification logits. This variant is produced when an exporter
includes the two softmax operations in the inference graph.

The L2CS-Net Gaze360 LiteRT artifact is a concrete example. Its model
card states that softmax is baked in, and the model graph ends with two
`SOFTMAX` operators, one producing each `[1, 90]` output.
An independent ONNX exporter also returns `torch.softmax(yaw)` and
`torch.softmax(pitch)` from `forward()` and names the exported outputs
`yaw_softmax` and `pitch_softmax`.

L2CS-Net Probability Output Tensors:

|Name|Shape|Description|
|---|---|---|
|[yaw]|1 x NUM_BINS|Normalized probabilities for horizontal (yaw) angle bins|
|[pitch]|1 x NUM_BINS|Normalized probabilities for vertical (pitch) angle bins|

## Tensor Decoding Logic

```
# The tensors are already softmax-normalized.
# Gaze360: NUM_BINS=90, BIN_WIDTH=4, ANGLE_OFFSET=180

idx = [0, 1, ..., NUM_BINS - 1]

yaw_degrees   = sum(yaw[i]   * idx[i] for i in 0:NUM_BINS-1) * BIN_WIDTH - ANGLE_OFFSET
pitch_degrees = sum(pitch[i] * idx[i] for i in 0:NUM_BINS-1) * BIN_WIDTH - ANGLE_OFFSET

yaw_radians   = yaw_degrees   * pi / 180
pitch_radians = pitch_degrees * pi / 180

gaze_x = -cos(pitch_radians) * sin(yaw_radians)
gaze_y = -sin(pitch_radians)
gaze_z = -cos(pitch_radians) * cos(yaw_radians)
```

Applying softmax again is incorrect because it changes an already
normalized distribution. The angle mapping also follows the official
L2CS-Net decoder exactly: `sum(p[i] * i) * BIN_WIDTH - ANGLE_OFFSET`,
without a half-bin offset.

One derivative ONNX export script uses geometric bin centers
(`i + 0.5`) in its optional validation helper. That does not change the
probability tensor encoding, and it differs by a constant half-bin
from the official L2CS-Net inference formula. It is therefore a
decoder variation, not another tensor-group variant.

## External References

* [L2CS-Net: Fine-Grained Gaze Estimation in Unconstrained Environments Paper](https://arxiv.org/abs/2203.03339)
* [Official L2CS-Net model returns logits](https://github.com/Ahmednull/L2CS-Net/blob/a4d8f7fa5436a2b2b9f088471623b552a85811bd/l2cs/model.py#L67-L70)
* [Official L2CS-Net decoder applies softmax and uses bin indices](https://github.com/Ahmednull/L2CS-Net/blob/a4d8f7fa5436a2b2b9f088471623b552a85811bd/l2cs/pipeline.py#L121-L131)
* [L2CS-Net LiteRT model card: probability outputs and baked-in softmax](https://huggingface.co/litert-community/L2CS-Gaze360-LiteRT/blob/5e1548c7976d95afd90cdb5dce1b5b8c8d515bfc/README.md)
* [Independent ONNX exporter with softmax outputs](https://github.com/thanhhung-dev/driver-monitoring-system/blob/267e33ef450ce443184c8aa0ca09a17ebb39a68a/scripts/export_l2cs.py#L39-L52)
* [Derivative half-bin validation helper](https://github.com/thanhhung-dev/driver-monitoring-system/blob/267e33ef450ce443184c8aa0ca09a17ebb39a68a/scripts/export_l2cs.py#L55-L63)

## Models

* [L2CS-Net Gaze360 LiteRT model artifact](https://huggingface.co/litert-community/L2CS-Gaze360-LiteRT/blob/5e1548c7976d95afd90cdb5dce1b5b8c8d515bfc/gaze_fp16.tflite)

## Tensor Decoders

|Framework|Links|
|---|---|
|LiteRT|[L2CS-Net LiteRT probability-output decoder](https://huggingface.co/litert-community/L2CS-Gaze360-LiteRT/blob/5e1548c7976d95afd90cdb5dce1b5b8c8d515bfc/README.md#minimal-usage)|

[l2cs-net-out]: /tensor-groups/l2cs-net-out.md
[yaw]: /tensors/hopenet-softmaxed-out-yaw.md
[pitch]: /tensors/hopenet-softmaxed-out-pitch.md
