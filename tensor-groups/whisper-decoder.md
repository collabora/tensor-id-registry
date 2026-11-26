# Whisper Decoder

* tensor-group: yes
* layer-type: inout
* use-case: automatic-speech-recognition

## Description

Runs the text decoder on the audio encoder's hidden states and a sequence
of token IDs. The corresponding GStreamer model-info group ID is
`whisper-decoder`.

| Tensor | Direction | Shape |
|---|---|---|
| [Hidden states][hidden] | Input | BATCH_SIZE x AUDIO_FRAMES x HIDDEN_SIZE |
| [Token IDs][ids] | Input | BATCH_SIZE x TOKEN_COUNT |
| [Logits][logits] | Output | BATCH_SIZE x TOKEN_COUNT x VOCAB_SIZE |
| [Cross-attention weights][cross-attention-weights], per layer | Optional output | BATCH_SIZE x HEADS x TOKEN_COUNT x AUDIO_FRAMES |
| Self-attention [key][self-key] and [value][self-value], per layer | Optional output | BATCH_SIZE x HEADS x TOKEN_COUNT x HEAD_SIZE |
| Cross-attention [key][cross-key] and [value][cross-value], per layer | Optional output | BATCH_SIZE x HEADS x AUDIO_FRAMES x HEAD_SIZE |

## Tensor Decoding Logic

```
# Autoregressive inference
tokens = initial prompt for the model, language and task
while the decoder context has room:
    logits = decoder(hidden_states, tokens)
    token = select from logits at the last valid input position
    if token is end-of-text:
        break
    append token to tokens

# Convert generated text token IDs to a string using the model tokenizer.
text = tokenizer.decode(generated text token IDs)
```

The selection step applies Whisper's token suppression and, when enabled,
timestamp rules. Timestamp token IDs encode offsets from the start of
the audio window in 20 ms increments and can mark segment boundaries.

The [AMD Small export][amd-small] uses a fixed token tensor and returns only logits.
The [timestamped Small export][timestamped-small] accepts a variable prefix length.
Its cross-attention weights support DTW token alignment, and its self- and
cross-attention key/value tensors initialize the
[decoder with KV cache](/tensor-groups/whisper-decoder-with-kv-cache.md).

## External References

* [Robust Speech Recognition via Large-Scale Weak Supervision](https://arxiv.org/abs/2212.04356)
* [Tokenization and timestamp tokens](https://github.com/openai/whisper/blob/main/whisper/tokenizer.py)
* [Whisper decoding and timestamp rules](https://github.com/openai/whisper/blob/main/whisper/decoding.py)
* [whisper.cpp text decoder](https://github.com/ggml-org/whisper.cpp/blob/master/src/whisper.cpp)

## Models

* [AMD Whisper Small ONNX export][amd-small]
* [Whisper Small timestamped ONNX export][timestamped-small]

## Tensor Decoders

| Framework | Links |
|---|---|
| OpenAI Whisper | [Reference implementation](https://github.com/openai/whisper/blob/main/whisper/decoding.py) |

[hidden]: /tensors/whisper-encoder-inout-hidden-states.md
[ids]: /tensors/whisper-decoder-in-token-ids.md
[logits]: /tensors/whisper-decoder-out-logits.md
[amd-small]: https://huggingface.co/amd/whisper-small-onnx-npu
[timestamped-small]: https://huggingface.co/onnx-community/whisper-small_timestamped/tree/main/onnx
[cross-attention-weights]: /tensors/whisper-decoder-out-cross-attention-weights.md
[self-key]: /tensors/whisper-decoder-inout-self-attention-key.md
[self-value]: /tensors/whisper-decoder-inout-self-attention-value.md
[cross-key]: /tensors/whisper-decoder-inout-cross-attention-key.md
[cross-value]: /tensors/whisper-decoder-inout-cross-attention-value.md
