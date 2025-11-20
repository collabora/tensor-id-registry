# Classification

- tensor-group: no
- layer-type: output
- use-case: face-detection
- part-of-tensor-groups:
    - [ultra-lightweight-face-detection-rfb-320-v1-without-postproc-out](/tensor-groups/ultra-lightweight-face-detection-rfb-320-v1-without-postproc-out.md)

# Description
Location of faces detected

## Boxes Tensor

- tensor-shape: 1 x COUNT x 4
- tensor-datatype: float32
- tensor-id: ultra-lightweight-face-detection-rfb-320-v1-variant-1-out-boxes-without-postproc

### Encoding

Scheme: (delta-center-x, delta-center-y, delta-log-width, delta-log-height)

The COUNT is variable and depends on the model training.

The COUNT can be retrieved from the tensor dimensions.

Each entry in the table is the variance on the matching anchor which
is also defined at training time. Each anchor is defined as the
following tuple:
`(anchor-center-x, anchor-center-y, anchor-width, anchor-height)`

|box-1 | box-1 | box-1 | box-1 | box-2 | box-2 | box-2 | box-2 | ... | COUNT|COUNT|COUNT|COUNT|
|---   |---    |---    |---    |---    |---    |---    |---    |---  |---                |---                |---                |---                |
| delta-center-x | delta-center-y | delta-log-width | delta-log-height | delta-center-x | delta-center-y | delta-log-width | delta-log-height | ... | delta-center-x | delta-center-y | delta-log-width | delta-log-height |

Memory layout of tensor data:

|Index                |Value              |
|---                  |---                |
| -                   | -                 |
|0                    |delta-center-x     |
|1                    |delta-center-y     |
|2                    |delta-log-width    |
|3                    |delta-log-height   |
|4                    |delta-center-x     |
|5                    |delta-center-y     |
|6                    |delta-log-width    |
|7                    |delta-log-height   |
|...                  | ...               |
|(COUNT - 1) x 4      |delta-center-x     |
|(COUNT - 1) x 4 + 1  |delta-center-y     |
|(COUNT - 1) x 4 + 2  |delta-log-width    |
|(COUNT - 1) x 4 + 3  |delta-log-height   |



# Models

* [Model source](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master)
* [ONNX pre-trained model](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master/models/)

# Tensor Decoders
|Framework | Links |
|---       |---    |
|GStreamer | [perm](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/blob/main/subprojects/gst-plugins-bad/gst/tensordecoders/gstfacedetectortensordecoder.c) |

