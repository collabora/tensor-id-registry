# CTC Text Recognition Output

* tensor-group: yes
* layer-type: output
* use-case: text-recognition

## Description

Output tensor group for text recognition models using Connectionist
Temporal Classification (CTC).

The output represents a sequence of predictions over a character or token
vocabulary. Each sequence position contains predictions for all classes,
including the CTC blank class.

The model output is either probabilities or logits, depending on the model.
Each instance of this group contains exactly one tensor: either prob or
logits. Both representations use the same batch, time-step, and class axes
and the same CTC decoding rules.

CTC Text Recognition Output Tensors:

| Name | Shape | Description |
|---|---|---|
| [prob] | BATCH_SIZE x SEQUENCE_LENGTH x NUM_CLASSES | Per-class probabilities for each sequence position |
| [logits] | BATCH_SIZE x SEQUENCE_LENGTH x NUM_CLASSES | Unnormalized per-class scores for each sequence position |

## Tensor Decoding Logic

For each sample in the batch, decode the output sequence using the configured
CTC decoding method.

The ordered vocabulary excludes blank and contains NUM_CLASSES - 1 tokens.
BLANK_INDEX and the vocabulary are supplied by the model or decoder
configuration, as described in the individual tensor definitions.

Greedy decoding uses the following procedure. If classes tie for the maximum,
select the lowest class index for this procedure.

```text
scores = the single prob or logits tensor
for b = 0 .. BATCH_SIZE - 1:
    previous = NONE
    text = ""
    for t = 0 .. SEQUENCE_LENGTH - 1:
        c = argmax(scores[b, t, :])
        if c != previous and c != BLANK_INDEX:
            token_index = c if c < BLANK_INDEX else c - 1
            text += vocabulary[token_index]
        previous = c
    result[b] = text
```

Updating previous even for blank preserves repeated classes separated by a
blank: A, blank, A produces AA, whereas A, A produces A. Blank does not
represent a space; a space must be an entry in the vocabulary.

Softmax is unnecessary for greedy class selection. For probability-based
CTC beam search, use prob directly or normalize logits over the class axis
at each time step. Log-domain decoders use log-probabilities instead.
Greedy decoding selects the most likely path; it does not necessarily find
the most likely text after summing the probabilities of equivalent paths.

## External References

* [Connectionist Temporal Classification: Labelling Unsegmented Sequence Data with Recurrent Neural Networks](https://www.cs.toronto.edu/~graves/icml_2006.pdf)

## Models

* [PaddleOCR English recognition ONNX model (monkt export)](https://huggingface.co/monkt/paddleocr-onnx/blob/7b02d0a30a07ba2b92ad1ff5a8941ae2c633de65/languages/english/rec.onnx)
* [EasyOCR recognizer, Qualcomm AI Hub ONNX float distribution](https://huggingface.co/qualcomm/EasyOCR#option-1-download-pre-exported-models)

## Tensor Decoders

| Framework | Links |
|---|---|
| PaddleOCR | [CTCLabelDecode](https://github.com/PaddlePaddle/PaddleOCR/blob/main/ppocr/postprocess/rec_postprocess.py) |
| EasyOCR | [CTCLabelConverter](https://github.com/JaidedAI/EasyOCR/blob/master/easyocr/utils.py) |

[prob]: /tensors/ctc-text-recognition-out-prob.md
[logits]: /tensors/ctc-text-recognition-out-logits.md
