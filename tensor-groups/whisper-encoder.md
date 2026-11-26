# Whisper Encoder

* tensor-group: yes
* layer-type: inout
* use-case: automatic-speech-recognition

## Description

The Whisper Transformer encoder converts normalized log-Mel features into
audio hidden states. The matching [Whisper decoder](/tensor-groups/whisper-decoder.md)
consumes these hidden states directly as the input to cross-attention.

The GStreamer model-info group ID is `whisper-encoder`.

| Tensor | Direction | Shape |
|---|---|---|
| [Input features][features] | Input | BATCH_SIZE x MEL_BINS x MEL_FRAMES |
| [Hidden states][hidden] | Output | BATCH_SIZE x AUDIO_FRAMES x HIDDEN_SIZE |

## External References

* [Robust Speech Recognition via Large-Scale Weak Supervision](https://arxiv.org/abs/2212.04356)
* [Audio preprocessing](https://github.com/openai/whisper/blob/main/whisper/audio.py)
* [Audio encoder](https://github.com/openai/whisper/blob/main/whisper/model.py)
* [whisper.cpp audio encoder](https://github.com/ggml-org/whisper.cpp/blob/master/src/whisper.cpp)

## Models

* [AMD Whisper Small ONNX export](https://huggingface.co/amd/whisper-small-onnx-npu)
* [Whisper Small timestamped ONNX export](https://huggingface.co/onnx-community/whisper-small_timestamped/tree/main/onnx)

[features]: /tensors/whisper-encoder-in-features.md
[hidden]: /tensors/whisper-encoder-inout-hidden-states.md
