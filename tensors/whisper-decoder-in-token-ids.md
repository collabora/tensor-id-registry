# Classification

* tensor-group: no
* layer-type: input
* use-case: automatic-speech-recognition
* part-of-tensor-groups:
  * [whisper-decoder](/tensor-groups/whisper-decoder.md)
  * [whisper-decoder-with-kv-cache](/tensor-groups/whisper-decoder-with-kv-cache.md)

# Description

## Whisper Decoder Input Token IDs

| Field | Value |
|---|---|
| tensor-shape | BATCH_SIZE x TOKEN_COUNT |
| tensor-datatype | int64 |
| tensor-id | whisper-decoder-in-token-ids |

### Known Aliases

* `input_ids` (timestamped export)
* `x` (AMD decoder export)

### Encoding

Each value is a zero-based ID in the tokenizer vocabulary paired with the
model. A sequence can include prompt, language, task, text, and timestamp
tokens. TOKEN_COUNT is the number of input positions in this call, including
any padding imposed by the export. The cached Small decoder consumes one
token per call. The AMD Small decoder takes a fixed 128 positions.

The tensor uses row-major order.

| Index | Value |
|---|---|
| 0 | First sample, first token ID |
| ... | ... |
| TOKEN_COUNT - 1 | First sample, last token ID |
| ... | ... |
| BATCH_SIZE × TOKEN_COUNT - 1 | Last sample, last token ID |

# External References

* [Robust Speech Recognition via Large-Scale Weak Supervision](https://arxiv.org/abs/2212.04356)
* [OpenAI Whisper implementation](https://github.com/openai/whisper/blob/main/whisper/decoding.py)

# Models

* [AMD Whisper Small ONNX export](https://huggingface.co/amd/whisper-small-onnx-npu)
* [Whisper Small timestamped ONNX export](https://huggingface.co/onnx-community/whisper-small_timestamped/tree/main/onnx)

# Tensor Decoders

| Framework | Links |
|---|---|
| OpenAI Whisper | [Prompt construction and token feedback](https://github.com/openai/whisper/blob/main/whisper/decoding.py) |
