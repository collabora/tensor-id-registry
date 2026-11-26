# Classification

* tensor-group: no
* layer-type: output
* use-case: automatic-speech-recognition
* part-of-tensor-groups:
  * [whisper-decoder](/tensor-groups/whisper-decoder.md)
  * [whisper-decoder-with-kv-cache](/tensor-groups/whisper-decoder-with-kv-cache.md)

# Description

## Whisper Decoder Cross-Attention Weights

| Field | Value |
|---|---|
| tensor-shape | BATCH_SIZE x HEADS x TOKEN_COUNT x AUDIO_FRAMES |
| tensor-datatype | float32 |
| tensor-id pattern | `whisper-decoder-out-cross-attention-weights.<layer>` |

Replace `<layer>` with the zero-based decoder layer index. For example,
`whisper-decoder-out-cross-attention-weights.0` identifies the tensor for layer zero.

### Known Aliases

In the timestamped ONNX export, `L` denotes the same zero-based layer index.

* `cross_attentions.L`

### Encoding

Each entry is a post-softmax weight from one decoder token position to one
encoder audio frame. For each sample, head, and token position, softmax is
applied across the audio frame axis. Use the model's alignment layer/head
pairs to select weights for dynamic time warping (DTW) token alignment.
TOKEN_COUNT is the number of token positions processed in the current call.

The tensor uses row-major order.

| Index | Value |
|---|---|
| 0 | First sample, first head, first token position, first audio frame |
| ... | ... |
| AUDIO_FRAMES - 1 | First sample, first head, first token position, last audio frame |
| ... | ... |
| BATCH_SIZE x HEADS x TOKEN_COUNT x AUDIO_FRAMES - 1 | Last sample, last head, last token position, last audio frame |

# External References

* [Robust Speech Recognition via Large-Scale Weak Supervision](https://arxiv.org/abs/2212.04356)
* [OpenAI Whisper implementation](https://github.com/openai/whisper/blob/main/whisper/timing.py)

# Models

* [Whisper Small timestamped ONNX export](https://huggingface.co/onnx-community/whisper-small_timestamped/tree/main/onnx)

# Tensor Decoders

| Framework | Links |
|---|---|
| OpenAI Whisper | [Token alignment](https://github.com/openai/whisper/blob/main/whisper/timing.py) |
