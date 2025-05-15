# Classification

- tensor-group: no
- layer-type: output
- use-case: face-detection
- part-of-tensor-groups:
    - [ultra-lightweight-face-detection-rfb-320-v1-variant-1-out](/tensor-groups/ultra-lightweight-face-detection-rfb-320-v1-variant-1-out.md)

# Description

Confidence levels of faces associated [bounding boxes location](/tensors/ultra-lightweight-face-detection-rfb-320-v1-variant-1-out-boxes.md)

## Scores Tensor

* tensor-shape: 1 x Num of Bounding Box predictions x 2
* tensor-datatype: float32
* tensor-id: out-ultra-lightweight-face-detection-rfb-320-v1-variant-1

### Encoding
Number of detected faces corresponding to the value stored in the tensor.

Memory layout of tensor data:

|Index                            |Value                                | Value                   |
|---                              |---                                  |---                      |
| -                               | -                                   | -                       | -                       |
|0                                | det-1-background-score              | det-1-foreground-score  |
|1                                | det-2-background-score              | det-2-foreground-score  |
|...                              | ...                                 | ...                     | ...                     |
|n - 1                            | det-n-1-background-score            | det-n-1-foreground-score|

# External References

* [Ultra Light Fast Face Detector](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB)
* [ONNX models](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master/models/onnx)

# Models

* [ONNX models](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master/models/onnx)

# Tensor Decoders
|Framework | Links |
|---       |---    |
|GStreamer | [latest]()https://gitlab.freedesktop.org/gstreamer/gstreamer/-/blame/main/subprojects/gst-plugins-bad/gst/tensordecoders/gstfacedetectortensordecoder.c?ref_type=heads#L38-42 |
