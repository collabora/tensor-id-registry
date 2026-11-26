# Classification

* tensor-group: no
* layer-type: inout
* use-case: automatic-speech-recognition
* part-of-tensor-groups:
  * [whisper-encoder](/tensor-groups/whisper-encoder.md)
  * [whisper-decoder](/tensor-groups/whisper-decoder.md)

# Description

## Whisper Encoder Hidden States

| Field | Value |
|---|---|
| tensor-shape | BATCH_SIZE x AUDIO_FRAMES x HIDDEN_SIZE |
| tensor-datatype | float32 |
| tensor-id | whisper-encoder-inout-hidden-states |

### Known Aliases

* `last_hidden_state` (timestamped encoder export)
* `encoder_hidden_states` (timestamped decoder export)
* `layer_norm_24` (AMD encoder export) and `xa` (AMD decoder export)

### Encoding

Each audio frame contains the encoder's final hidden vector. The encoder
reduces the Mel frame rate by a factor of two, so 3,000 Mel frames produce
1,500 encoder positions at 20 ms intervals. HIDDEN_SIZE is the encoder
width, which is 768 for Whisper Small. Pass the tensor directly to a
compatible decoder as the source for cross-attention.

The tensor uses row-major order.

| Index | Value |
|---|---|
| 0 | First sample, first audio frame, first hidden feature |
| ... | ... |
| HIDDEN_SIZE - 1 | First sample, first audio frame, last hidden feature |
| ... | ... |
| BATCH_SIZE × AUDIO_FRAMES × HIDDEN_SIZE - 1 | Last sample, last audio frame, last hidden feature |

# External References

* [Robust Speech Recognition via Large-Scale Weak Supervision](https://arxiv.org/abs/2212.04356)
* [OpenAI Whisper implementation](https://github.com/openai/whisper/blob/main/whisper/model.py)

# Models

* [AMD Whisper Small ONNX export](https://huggingface.co/amd/whisper-small-onnx-npu)
* [Whisper Small timestamped ONNX export](https://huggingface.co/onnx-community/whisper-small_timestamped/tree/main/onnx)

# Tensor Decoders

| Framework | Links |
|---|---|
| OpenAI Whisper | [AudioEncoder and TextDecoder](https://github.com/openai/whisper/blob/main/whisper/model.py) |
