# Hopenet Pitch Bin Probabilities

- tensor-group: no
- layer-type: output
- use-case: head-pose-estimation, gaze-estimation
- part-of-tensor-groups: [l2cs-net-softmaxed-out]

# Description

Vector of normalized probabilities for discretized pitch (vertical
rotation) bins. This is the post-softmax derivative of
[hopenet-out-pitch] and uses the same encoding as
[hopenet-softmaxed-out-yaw], differing only in the physical angle
represented.

## Pitch Bin Probabilities Tensor

|tensor-shape|tensor-datatype|tensor-id|
|---|---|---|
|1 x NUM_BINS|float32|hopenet-softmaxed-out-pitch|

## Known Aliases

* pitch
* pitch probabilities
* pitch_probs

### Encoding

Every element is finite and non-negative, and all elements sum to
approximately 1:

```
probabilities[i] = softmax(logits)[i]
angle_degrees = sum(probabilities[i] * i for i in 0:NUM_BINS-1) * BIN_WIDTH - ANGLE_OFFSET
```

Memory layout of tensor data:

|Index|Value|
|---|---|
|0|probability for pitch bin 0|
|1|probability for pitch bin 1|
|...|...|
|NUM_BINS - 1|probability for pitch bin NUM_BINS - 1|

# External References

* [Fine-Grained Head Pose Estimation Without Keypoints (Hopenet) Paper](https://arxiv.org/abs/1710.00925)
* [L2CS-Net: Fine-Grained Gaze Estimation in Unconstrained Environments Paper](https://arxiv.org/abs/2203.03339)
* [Official L2CS-Net softmax and expectation decoder](https://github.com/Ahmednull/L2CS-Net/blob/a4d8f7fa5436a2b2b9f088471623b552a85811bd/l2cs/pipeline.py#L121-L131)
* [L2CS-Net LiteRT model card: softmax is included in the graph](https://huggingface.co/litert-community/L2CS-Gaze360-LiteRT/blob/5e1548c7976d95afd90cdb5dce1b5b8c8d515bfc/README.md)
* [Independent ONNX exporter with a softmax pitch output](https://github.com/thanhhung-dev/driver-monitoring-system/blob/267e33ef450ce443184c8aa0ca09a17ebb39a68a/scripts/export_l2cs.py#L39-L52)

# Models

* [L2CS-Net Gaze360 LiteRT model artifact](https://huggingface.co/litert-community/L2CS-Gaze360-LiteRT/blob/5e1548c7976d95afd90cdb5dce1b5b8c8d515bfc/gaze_fp16.tflite)

# Tensor Decoders

|Framework|Links|
|---|---|
|LiteRT|[L2CS-Net LiteRT probability-output decoder](https://huggingface.co/litert-community/L2CS-Gaze360-LiteRT/blob/5e1548c7976d95afd90cdb5dce1b5b8c8d515bfc/README.md#minimal-usage)|

[hopenet-out-pitch]: /tensors/hopenet-out-pitch.md
[hopenet-softmaxed-out-yaw]: /tensors/hopenet-softmaxed-out-yaw.md
