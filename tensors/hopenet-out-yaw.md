# Hopenet Yaw Bins

- tensor-group: no
- layer-type: output
- use-case: head-pose-estimation, gaze-estimation
- part-of-tensor-groups: [l2cs-net-out]

# Description

Vector of per-bin classification logits representing a discretized
yaw (horizontal rotation) angle. The continuous angle is recovered by
applying [softmax](https://en.wikipedia.org/wiki/Softmax_function) to
the logits and computing the expected value over the bin indices,
then mapping that expectation to degrees using the bin width and
angle offset:

```
angle_degrees = sum(softmax(logits)[i] * i for i in 0:NUM_BINS-1) * BIN_WIDTH - ANGLE_OFFSET
```

The model output is specifically the pre-softmax value. See
[hopenet-softmaxed-out-yaw] for exports that include softmax in the
graph and therefore output normalized probabilities.

This "fine-grained classification" encoding, where a continuous
rotation angle is predicted as a softmax-weighted combination of
discrete angle bins instead of direct regression, was introduced by
Hopenet for head pose estimation (yaw/pitch/roll, 3 tensors) and was
later reused unmodified (dropping the roll tensor) by L2CS-Net for
gaze estimation. See [l2cs-net-out] for the yaw + pitch tensor-group
built from this tensor.

**This tensor is byte-for-byte identical in shape, datatype and decode
formula to [hopenet-out-pitch]** — the two only differ in which FC
head produced them and which physical angle the decoded value is
labeled as. A decoder implementation only needs to be written once and
reused for both tensors (and for Hopenet's roll tensor, which follows
the same encoding but is not documented separately in this registry).

## Yaw Bins Tensor

|tensor-shape | tensor-datatype | tensor-id |
|---|---|---|
|1 x NUM_BINS | float32 | hopenet-out-yaw |

## Known Aliases
* yaw
* pre_yaw
* fc_yaw / fc_yaw_gaze
* gaze_yaw

### Encoding

`NUM_BINS`, `BIN_WIDTH` (degrees per bin) and `ANGLE_OFFSET` (degrees)
are model/dataset dependent, and identical across yaw/pitch (and
roll) for a given model. For example:
* Hopenet's original head-pose model (300W-LP/BIWI/AFLW2000):
  `NUM_BINS=66`, `BIN_WIDTH=3`, `ANGLE_OFFSET=99`.
* L2CS-Net and derived gaze models trained on Gaze360:
  `NUM_BINS=90`, `BIN_WIDTH=4`, `ANGLE_OFFSET=180`.
* MobileGaze (yakhyo/gaze-estimation) trained on MPIIGaze:
  `NUM_BINS=28`, `BIN_WIDTH=3`, `ANGLE_OFFSET=42`.

Memory layout of tensor data:

|Index | Value |
|---|---|
|0 | classification logit for yaw bin 0|
|1 | classification logit for yaw bin 1|
|...|...|
|NUM_BINS - 1 | classification logit for yaw bin NUM_BINS - 1|

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

[l2cs-net-out]: /tensor-groups/l2cs-net-out.md
[hopenet-out-pitch]: /tensors/hopenet-out-pitch.md
[hopenet-softmaxed-out-yaw]: /tensors/hopenet-softmaxed-out-yaw.md
