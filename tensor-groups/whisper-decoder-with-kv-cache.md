# Whisper Decoder with KV Cache

* tensor-group: yes
* layer-type: inout
* use-case: automatic-speech-recognition

## Description

Consumes one token and the attention cache from earlier decoding steps.
The corresponding GStreamer model-info group ID is
`whisper-decoder-with-kv-cache`. The additional entries below describe the
timestamped ONNX export's layer-indexed interface.

| Tensor | Direction | Shape |
|---|---|---|
| [Token IDs][ids] | Input | BATCH_SIZE x 1 |
| [Logits][logits] | Output | BATCH_SIZE x 1 x VOCAB_SIZE |
| Self-attention [key][self-key] and [value][self-value], per layer | Input and output | BATCH_SIZE x HEADS x TEXT_POSITIONS x HEAD_SIZE |
| Cross-attention [key][cross-key] and [value][cross-value], per layer | Input | BATCH_SIZE x HEADS x AUDIO_FRAMES x HEAD_SIZE |
| [Cross-attention weights][cross-attention-weights], per layer | Optional output | BATCH_SIZE x HEADS x TOKEN_COUNT x AUDIO_FRAMES |

## Tensor Decoding Logic

```
# Build the prompt and run the initial decoder once.
prompt = initial prompt for the model, language and task
logits, cache = decoder(hidden_states, prompt)
generated = []

# Feed one token at a time to the decoder with KV cache.
while the decoder context has room:
    token = select from logits at the last valid input position
    if token is end-of-text:
        break
    append token to generated
    logits, cache = decoder_with_kv_cache(token, cache)

# Convert generated text token IDs to a string as in decoding without a cache.
text = tokenizer.decode(generated text token IDs)
```

The [initial decoder](/tensor-groups/whisper-decoder.md) produces both caches.
Each cached call adds one position to the self-attention cache and reuses the
same cross-attention cache for the audio window.

Each cache tensor uses the same layer-indexed ID on the decoder output and
the corresponding cached decoder input. See the tensor definitions for
the ONNX names used by the timestamped export.

The timestamped Small export includes cross-attention weights as an output
for DTW token alignment. Other exports may omit this output.
TOKEN_COUNT is one in the single-token decoder. Whisper Small has 12
decoder layers and 12 heads of width 64.

## External References

* [Robust Speech Recognition via Large-Scale Weak Supervision](https://arxiv.org/abs/2212.04356)
* [Whisper attention-cache handling](https://github.com/openai/whisper/blob/main/whisper/model.py)
* [Whisper reference token alignment](https://github.com/openai/whisper/blob/main/whisper/timing.py)
* [whisper.cpp KV cache and DTW alignment](https://github.com/ggml-org/whisper.cpp/blob/master/src/whisper.cpp)

## Models

* [AMD Whisper Small ONNX export](https://huggingface.co/amd/whisper-small-onnx-npu)
* [Whisper Small timestamped ONNX export](https://huggingface.co/onnx-community/whisper-small_timestamped/tree/main/onnx)

## Tensor Decoders

| Framework | Links |
|---|---|
| OpenAI Whisper | [Reference implementation](https://github.com/openai/whisper/blob/main/whisper/model.py) |

[ids]: /tensors/whisper-decoder-in-token-ids.md
[logits]: /tensors/whisper-decoder-out-logits.md
[cross-attention-weights]: /tensors/whisper-decoder-out-cross-attention-weights.md
[self-key]: /tensors/whisper-decoder-inout-self-attention-key.md
[self-value]: /tensors/whisper-decoder-inout-self-attention-value.md
[cross-key]: /tensors/whisper-decoder-inout-cross-attention-key.md
[cross-value]: /tensors/whisper-decoder-inout-cross-attention-value.md
