# Classification

* tensor-group: no
* layer-type: inout
* use-case: automatic-speech-recognition
* part-of-tensor-groups:
  * [whisper-decoder](/tensor-groups/whisper-decoder.md)
  * [whisper-decoder-with-kv-cache](/tensor-groups/whisper-decoder-with-kv-cache.md)

# Description

## Whisper Decoder Self-Attention Cached Keys

| Field | Value |
|---|---|
| tensor-shape | BATCH_SIZE x HEADS x TEXT_POSITIONS x HEAD_SIZE |
| tensor-datatype | float32 |
| tensor-id pattern | `whisper-decoder-inout-self-attention-key.<layer>` |

Replace `<layer>` with the zero-based decoder layer index. For example,
`whisper-decoder-inout-self-attention-key.0` identifies the tensor for layer zero.

### Known Aliases

In the timestamped ONNX export, `L` denotes the same zero-based layer index.

* `present.L.decoder.key` (output)
* `past_key_values.L.decoder.key` (input)

### Encoding

Each entry is a component of a self-attention key projection for one
text position of one decoder head. Key projections are compared with the decoder queries to compute attention scores.

The initial decoder fills the cache from the prompt. Each cached run
appends one text position. The input sequence axis counts the positions
already cached, and the output axis includes the newly processed token.

The tensor uses row-major order.

| Index | Value |
|---|---|
| 0 | First sample, first head, first text position, first component |
| ... | ... |
| HEAD_SIZE - 1 | First sample, first head, first text position, last component |
| ... | ... |
| BATCH_SIZE x HEADS x TEXT_POSITIONS x HEAD_SIZE - 1 | Last sample, last head, last text position, last component |

# External References

* [Robust Speech Recognition via Large-Scale Weak Supervision](https://arxiv.org/abs/2212.04356)
* [OpenAI Whisper implementation](https://github.com/openai/whisper/blob/main/whisper/model.py)

# Models

* [Whisper Small timestamped ONNX export](https://huggingface.co/onnx-community/whisper-small_timestamped/tree/main/onnx)

# Tensor Decoders

| Framework | Links |
|---|---|
| OpenAI Whisper | [Attention and KV cache](https://github.com/openai/whisper/blob/main/whisper/model.py) |
