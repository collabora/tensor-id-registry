# Classification

* tensor-group: no
* layer-type: output
* use-case: automatic-speech-recognition
* part-of-tensor-groups:
  * [whisper-decoder](/tensor-groups/whisper-decoder.md)
  * [whisper-decoder-with-kv-cache](/tensor-groups/whisper-decoder-with-kv-cache.md)

# Description

## Whisper Decoder Logits

| Field | Value |
|---|---|
| tensor-shape | BATCH_SIZE x TOKEN_COUNT x VOCAB_SIZE |
| tensor-datatype | float32 |
| tensor-id | whisper-decoder-out-logits |

### Known Aliases

* `logits` (timestamped export)
* `matmul` (AMD decoder export)

### Encoding

Each input position has a row of unnormalized next-token scores. The
vocabulary axis uses the decoder tokenizer's token IDs, including special
and timestamp tokens. VOCAB_SIZE is 51,865 in the two multilingual Small
exports and varies with the checkpoint. A full-prefix decoder uses the row
at the last valid input position for next-token selection. With a single
cached input token, use the only row. Apply the model's decoding rules
before selecting a token.

The tensor uses row-major order.

| Index | Value |
|---|---|
| 0 | First sample, first input position, first vocabulary score |
| ... | ... |
| VOCAB_SIZE - 1 | First sample, first input position, last vocabulary score |
| ... | ... |
| BATCH_SIZE × TOKEN_COUNT × VOCAB_SIZE - 1 | Last sample, last input position, last vocabulary score |

# External References

* [Robust Speech Recognition via Large-Scale Weak Supervision](https://arxiv.org/abs/2212.04356)
* [OpenAI Whisper implementation](https://github.com/openai/whisper/blob/main/whisper/decoding.py)

# Models

* [AMD Whisper Small ONNX export](https://huggingface.co/amd/whisper-small-onnx-npu)
* [Whisper Small timestamped ONNX export](https://huggingface.co/onnx-community/whisper-small_timestamped/tree/main/onnx)

# Tensor Decoders

| Framework | Links |
|---|---|
| OpenAI Whisper | [Next-token selection and timestamp rules](https://github.com/openai/whisper/blob/main/whisper/decoding.py) |
