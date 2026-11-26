# Classification

* tensor-group: no
* layer-type: inout
* use-case: automatic-speech-recognition
* part-of-tensor-groups:
  * [whisper-decoder](/tensor-groups/whisper-decoder.md)
  * [whisper-decoder-with-kv-cache](/tensor-groups/whisper-decoder-with-kv-cache.md)

# Description

## Whisper Decoder Cross-Attention Cached Keys

| Field | Value |
|---|---|
| tensor-shape | BATCH_SIZE x HEADS x AUDIO_FRAMES x HEAD_SIZE |
| tensor-datatype | float32 |
| tensor-id pattern | `whisper-decoder-inout-cross-attention-key.<layer>` |

Replace `<layer>` with the zero-based decoder layer index. For example,
`whisper-decoder-inout-cross-attention-key.0` identifies the tensor for layer zero.

### Known Aliases

In the timestamped ONNX export, `L` denotes the same zero-based layer index.

* `present.L.encoder.key` (output)
* `past_key_values.L.encoder.key` (input)

### Encoding

Each entry is a component of a cross-attention key projection for one
audio frame of one decoder head. Key projections are compared with the decoder queries to compute attention scores.

The initial decoder projects the encoder hidden states once per audio
window. The cached decoder reuses the same tensor on each subsequent run.

The tensor uses row-major order.

| Index | Value |
|---|---|
| 0 | First sample, first head, first audio frame, first component |
| ... | ... |
| HEAD_SIZE - 1 | First sample, first head, first audio frame, last component |
| ... | ... |
| BATCH_SIZE x HEADS x AUDIO_FRAMES x HEAD_SIZE - 1 | Last sample, last head, last audio frame, last component |

# External References

* [Robust Speech Recognition via Large-Scale Weak Supervision](https://arxiv.org/abs/2212.04356)
* [OpenAI Whisper implementation](https://github.com/openai/whisper/blob/main/whisper/model.py)

# Models

* [Whisper Small timestamped ONNX export](https://huggingface.co/onnx-community/whisper-small_timestamped/tree/main/onnx)

# Tensor Decoders

| Framework | Links |
|---|---|
| OpenAI Whisper | [Attention and KV cache](https://github.com/openai/whisper/blob/main/whisper/model.py) |
