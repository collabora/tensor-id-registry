# Classification

- tensor-group: no
- layer-type: output
- use-case: text-recognition
- part-of-tensor-groups:
    - [ctc-text-recognition-out](/tensor-groups/ctc-text-recognition-out.md)

# Description

Unnormalized per-class scores (logits) for each time step of a CTC text recognition
output sequence, including the CTC blank class.

See [ctc-text-recognition-out-prob] for the normalized probabilities
representation of the same output.

## CTC Text Recognition Logits Tensor

- tensor-shape: BATCH_SIZE x SEQUENCE_LENGTH x NUM_CLASSES
- tensor-datatype: float32
- tensor-id: ctc-text-recognition-out-logits
- memory-layout: row-major order

Where:
 * BATCH_SIZE is the number of input samples.
 * SEQUENCE_LENGTH is the number of output time steps per sample.
 * NUM_CLASSES is the number of vocabulary entries plus one CTC blank class.

### Known Aliases
* output_preds (EasyOCR ONNX model)

### Encoding

The dimensions are ordered as batch, time step, and class, with the class
dimension varying fastest.

Each element contains an unnormalized class score (logit). Applying softmax
over the class dimension at each time step gives the probability
representation described by [ctc-text-recognition-out-prob].

The model or decoder configuration supplies BLANK_INDEX and an ordered
vocabulary of NUM_CLASSES - 1 tokens, excluding blank. BLANK_INDEX is a
zero-based class index in [0, NUM_CLASSES - 1]. For a non-blank class c,
the vocabulary index is c if c < BLANK_INDEX, and c - 1 otherwise.

Memory layout of tensor data:

| Index | Value |
|---|---|
| 0 | First sample, first time step, first class logit |
| ... | ... |
| NUM_CLASSES - 1 | First sample, first time step, last class logit |
| ... | ... |
| SEQUENCE_LENGTH x NUM_CLASSES - 1 | First sample, last time step, last class logit |
| ... | ... |
| BATCH_SIZE x SEQUENCE_LENGTH x NUM_CLASSES - 1 | Last sample, last time step, last class logit |

See [CTC Text Recognition Output](/tensor-groups/ctc-text-recognition-out.md)
for decoding logic.

# External References

* [Connectionist Temporal Classification: Labelling Unsegmented Sequence Data with Recurrent Neural Networks](https://www.cs.toronto.edu/~graves/icml_2006.pdf)
* [An End-to-End Trainable Neural Network for Image-based Sequence Recognition and Its Application to Scene Text Recognition](https://arxiv.org/abs/1507.05717)
* [EasyOCR recognition model](https://github.com/JaidedAI/EasyOCR/blob/master/easyocr/model/vgg_model.py)

# Models

* [EasyOCR recognizer, Qualcomm AI Hub ONNX float distribution](https://huggingface.co/qualcomm/EasyOCR#option-1-download-pre-exported-models)
* [Original EasyOCR implementation](https://github.com/JaidedAI/EasyOCR)

# Tensor Decoders

| Framework | Links |
|---|---|
| EasyOCR | [Recognition and probability conversion](https://github.com/JaidedAI/EasyOCR/blob/master/easyocr/recognition.py), [CTCLabelConverter](https://github.com/JaidedAI/EasyOCR/blob/master/easyocr/utils.py) |

[ctc-text-recognition-out-prob]: /tensors/ctc-text-recognition-out-prob.md
