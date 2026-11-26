# Classification

* tensor-group: no
* layer-type: input
* use-case: automatic-speech-recognition
* part-of-tensor-groups:
  * [whisper-encoder](/tensor-groups/whisper-encoder.md)

# Description

## Whisper Encoder Input Features

| Field | Value |
|---|---|
| tensor-shape | BATCH_SIZE x MEL_BINS x MEL_FRAMES |
| tensor-datatype | float32 |
| tensor-id | whisper-encoder-in-features |

### Known Aliases

* `input_features` (timestamped export)
* `x` (AMD encoder export)

### Encoding

Each sample contains a normalized log-Mel power spectrogram computed from
mono 16 kHz audio. Use a Hann window of 400 samples and a hop of 160 samples
with a centered, reflect-padded STFT. Drop the last STFT time frame,
square the magnitudes, and apply Whisper's Slaney-normalized Mel filters.
Clamp the Mel power to at least `1e-10`, take its base-10 logarithm, floor
the result at the sample maximum minus 8, then apply `(value + 4) / 4`.
Typical Whisper models take 3,000 frames for a 30-second audio window.
Whisper Small uses 80 Mel bins, while Whisper Large v3 uses 128.

The tensor uses row-major order.

| Index | Value |
|---|---|
| 0 | First sample, first Mel bin, first frame |
| ... | ... |
| MEL_FRAMES - 1 | First sample, first Mel bin, last frame |
| MEL_FRAMES | First sample, second Mel bin, first frame |
| ... | ... |
| BATCH_SIZE × MEL_BINS × MEL_FRAMES - 1 | Last sample, last Mel bin, last frame |

# External References

* [Robust Speech Recognition via Large-Scale Weak Supervision](https://arxiv.org/abs/2212.04356)
* [OpenAI Whisper implementation](https://github.com/openai/whisper/blob/main/whisper/audio.py)

# Models

* [AMD Whisper Small ONNX export](https://huggingface.co/amd/whisper-small-onnx-npu)
* [Whisper Small timestamped ONNX export](https://huggingface.co/onnx-community/whisper-small_timestamped/tree/main/onnx)

# Tensor Decoders

| Framework | Links |
|---|---|
| OpenAI Whisper | [Log-Mel feature extraction](https://github.com/openai/whisper/blob/main/whisper/audio.py) |
