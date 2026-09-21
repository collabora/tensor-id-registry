# Hopenet Pitch Bins

- tensor-group: no
- layer-type: output
- use-case: head-pose-estimation, gaze-estimation
- part-of-tensor-groups: [l2cs-net-out]

# Description

Vector of per-bin classification logits representing a discretized
pitch (vertical rotation) angle.

The model output is specifically the pre-softmax value. See
[hopenet-softmaxed-out-pitch] for exports that include softmax in the
graph and therefore output normalized probabilities.

This tensor uses the exact same encoding as [hopenet-out-yaw],
same shape, datatype, bin/softmax/expected-value decode formula, and
`NUM_BINS`/`BIN_WIDTH`/`ANGLE_OFFSET` semantics. It only differs from
`hopenet-out-yaw` in which FC head produced it and which physical
angle the decoded value represents. See [hopenet-out-yaw] for the
full description of the encoding, decode formula and origin
(Hopenet, reused unmodified by L2CS-Net).

## Pitch Bins Tensor

|tensor-shape | tensor-datatype | tensor-id |
|---|---|---|
|1 x NUM_BINS | float32 | hopenet-out-pitch |

## Known Aliases
* pitch
* pre_pitch
* fc_pitch / fc_pitch_gaze
* gaze_pitch

### Encoding

Identical to [hopenet-out-yaw]'s encoding (same `NUM_BINS`,
`BIN_WIDTH` and `ANGLE_OFFSET` for a given model):

```
angle_degrees = sum(softmax(logits)[i] * i for i in 0:NUM_BINS-1) * BIN_WIDTH - ANGLE_OFFSET
```

Memory layout of tensor data:

|Index | Value |
|---|---|
|0 | classification logit for pitch bin 0|
|1 | classification logit for pitch bin 1|
|...|...|
|NUM_BINS - 1 | classification logit for pitch bin NUM_BINS - 1|

# External References

* [Fine-Grained Head Pose Estimation Without Keypoints (Hopenet) Paper](https://arxiv.org/abs/1710.00925)
* [Hopenet reference implementation](https://github.com/natanielruiz/deep-head-pose/blob/master/code/hopenet.py)
* [L2CS-Net: Fine-Grained Gaze Estimation in Unconstrained Environments Paper](https://arxiv.org/abs/2203.03339)
* [L2CS-Net reference implementation](https://github.com/Ahmednull/L2CS-Net/blob/main/l2cs/model.py)

# Models

* [Hopenet pretrained head-pose models](https://github.com/natanielruiz/deep-head-pose#pre-trained-models)
* [L2CS-Net pretrained gaze models](https://github.com/Ahmednull/L2CS-Net#model)
* [MobileGaze (yakhyo/gaze-estimation) MobileNetV2 ONNX export script](https://github.com/yakhyo/gaze-estimation/blob/main/onnx_export.py)

# Tensor Decoders
|Framework | Links |
|---|---|
|pytorch | [Hopenet test_hopenet.py decode](https://github.com/natanielruiz/deep-head-pose/blob/master/code/test_hopenet.py) |
|pytorch | [L2CS-Net Pipeline.predict_gaze](https://github.com/Ahmednull/L2CS-Net/blob/main/l2cs/pipeline.py) |
|onnxruntime | [MobileGaze GazeEstimationONNX.decode](https://github.com/yakhyo/gaze-estimation/blob/main/onnx_inference.py) |

[hopenet-out-yaw]: /tensors/hopenet-out-yaw.md
[l2cs-net-out]: /tensor-groups/l2cs-net-out.md
[hopenet-softmaxed-out-pitch]: /tensors/hopenet-softmaxed-out-pitch.md
