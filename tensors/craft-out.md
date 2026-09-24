# CRAFT Output

- tensor-group: no
- layer-type: output
- use-case: text-detection

# Description

Text and character-link score maps produced by CRAFT (Character Region
Awareness for Text detection) models.

## CRAFT Output Tensor

| Property | Value |
|---|---|
| tensor-shape | BATCH_SIZE x HEIGHT x WIDTH x 2 |
| tensor-datatype | float32 |
| tensor-id | craft-out |

Where:
* BATCH_SIZE is the number of input images.
* HEIGHT and WIDTH are the spatial dimensions of the output score maps.
  For the original CRAFT architecture, the score maps have half the spatial
  resolution of the model input.
* The last dimension contains the text score followed by the link score.

### Known Aliases

* `results` (Qualcomm EasyOCR ONNX float distribution)

### Encoding

The dimensions are ordered from outermost to innermost as batch, height, width,
and score. Data is stored in row-major order, with the score dimension varying
fastest.

For each input image, the output score maps form a HEIGHT x WIDTH grid. At each
spatial position, the first value is the text score and the second value is the
link score.

The text score indicates how strongly the model identifies a character at that
position. Scores are highest near the center of a character and decrease toward
its edges. The link score indicates how strongly the model identifies a
connection between adjacent characters in the same word at that position. Higher
link scores indicate stronger connections, while lower scores indicate weaker
connections. During post-processing, text and link scores are thresholded, and
connected regions are grouped into text detections.

Memory layout of tensor data:

| Index | Value |
|---|---|
| 0 | First sample, first spatial position, text score |
| 1 | First sample, first spatial position, link score |
| ... | ... |
| 2 x (HEIGHT x WIDTH - 1) | First sample, last spatial position, text score |
| 2 x (HEIGHT x WIDTH - 1) + 1 | First sample, last spatial position, link score |
| ... | ... |
| BATCH_SIZE x HEIGHT x WIDTH x 2 - 2 | Last sample, last spatial position, text score |
| BATCH_SIZE x HEIGHT x WIDTH x 2 - 1 | Last sample, last spatial position, link score |

Coordinates in the score maps are scaled by 2 when mapped back to the
CRAFT model input coordinate space.

CRAFT post-processing thresholds the text and link score maps and uses
connected components to produce text detection regions.

# External References

* [Character Region Awareness for Text Detection](https://arxiv.org/abs/1904.01941)
* [CRAFT-pytorch](https://github.com/clovaai/CRAFT-pytorch)
* [EasyOCR](https://github.com/JaidedAI/EasyOCR)

# Models

* [EasyOCR CRAFT detector, Qualcomm AI Hub ONNX float distribution](https://huggingface.co/qualcomm/EasyOCR)

# Tensor Decoders

| Framework | Links |
|---|---|
| CRAFT | [getDetBoxes_core](https://github.com/clovaai/CRAFT-pytorch/blob/master/craft_utils.py) |