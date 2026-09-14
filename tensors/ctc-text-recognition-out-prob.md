# Classification

- tensor-group: no
- layer-type: output
- use-case: text-recognition
- part-of-tensor-groups:
    - [ctc-text-recognition-out](/tensor-groups/ctc-text-recognition-out.md)

# Description

Per-class probabilities for each time step of a CTC text recognition
output sequence, including the CTC blank class.

See [ctc-text-recognition-out-logits] for the unnormalized logits
representation of the same output.

## CTC Text Recognition Probability Tensor

- tensor-shape: BATCH_SIZE x SEQUENCE_LENGTH x NUM_CLASSES
- tensor-datatype: float32
- tensor-id: ctc-text-recognition-out-prob
- memory-layout: row-major order

Where:
 * BATCH_SIZE is the number of input samples.
 * SEQUENCE_LENGTH is the number of output time steps per sample.
 * NUM_CLASSES is the number of vocabulary entries plus one CTC blank class.

### Known Aliases
* fetch_name_0 (PaddleOCR ONNX model)

### Encoding

The dimensions are ordered as batch, time step, and class, with the class
dimension varying fastest.

Each element contains a class probability in [0, 1]. At each time step,
probabilities across all classes, including blank, sum to 1 within
floating-point precision.

The model or decoder configuration supplies BLANK_INDEX and an ordered
vocabulary of NUM_CLASSES - 1 tokens, excluding blank. BLANK_INDEX is a
zero-based class index in [0, NUM_CLASSES - 1]. For a non-blank class c,
the vocabulary index is c if c < BLANK_INDEX, and c - 1 otherwise.

Memory layout of tensor data:

| Index | Value |
|---|---|
| 0 | First sample, first time step, first class probability |
| ... | ... |
| NUM_CLASSES - 1 | First sample, first time step, last class probability |
| ... | ... |
| SEQUENCE_LENGTH x NUM_CLASSES - 1 | First sample, last time step, last class probability |
| ... | ... |
| BATCH_SIZE x SEQUENCE_LENGTH x NUM_CLASSES - 1 | Last sample, last time step, last class probability |

See [CTC Text Recognition Output](/tensor-groups/ctc-text-recognition-out.md)
for decoding logic.

# External References

* [Connectionist Temporal Classification: Labelling Unsegmented Sequence Data with Recurrent Neural Networks](https://www.cs.toronto.edu/~graves/icml_2006.pdf)
* [An End-to-End Trainable Neural Network for Image-based Sequence Recognition and Its Application to Scene Text Recognition](https://arxiv.org/abs/1507.05717)
* [PaddleOCR CTC output head](https://github.com/PaddlePaddle/PaddleOCR/blob/main/ppocr/modeling/heads/rec_ctc_head.py)

# Models

* [PaddleOCR English recognition ONNX model (monkt export)](https://huggingface.co/monkt/paddleocr-onnx/blob/7b02d0a30a07ba2b92ad1ff5a8941ae2c633de65/languages/english/rec.onnx)

# Tensor Decoders

| Framework | Links |
|---|---|
| PaddleOCR | [CTCLabelDecode](https://github.com/PaddlePaddle/PaddleOCR/blob/main/ppocr/postprocess/rec_postprocess.py) |

[ctc-text-recognition-out-logits]: /tensors/ctc-text-recognition-out-logits.md
