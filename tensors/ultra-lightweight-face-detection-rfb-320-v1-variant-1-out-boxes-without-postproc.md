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

Scheme: (top left X, top left Y, width, height)

The COUNT is variable and depends on the model training.

Other constants are extracted from the training process:
- CENTER_VARIANCE = 0.1
- SIZE_VARIANCE = 0.2
- COUNT

The COUNT can be retrieved from the tensor dimensions.

Each entry in the table is the variance on the matching anchor which
is also defined at training time. Each anchor is defined as the
following tuple:
`(anchor-center-x, anchor-center-y, anchor-width anchor-height)`

Once the following formulas have been applied, the bounding boxes
center is calculated along with its size.

```
bounding-box-center-x = top-left-x * CENTER_VARIANCE * anchor-width + anchor-center-x
bounding-box-center-y = top-left-y * CENTER_VARIANCE * anchor-height + anchor-center-y
bounding-box-width = expf (width * SIZE_VARIANCE) * anchor-width
bounding-box-height = expf (height * SIZE_VARIANCE) * anchor-height
```

|box-1 | box-1 | box-1 | box-1 | box-2 | box-2 | box-2 | box-2 | ... | COUNT|COUNT|COUNT|COUNT|
|---   |---    |---    |---    |---    |---    |---    |---    |---  |---                |---                |---                |---                |
| top-left-X | top-left-Y | width | height | top-left-X | top-left-Y | width | height | ...  top-left-X | top-left-Y | width | height |

Memory layout of tensor data:

|Index                |Value              |
|---                  |---                |
| -                   | -                 |
|0                    |top-left-x         |
|1                    |top-left-y         |
|2                    |width              |
|3                    |height             |
|4                    |top-left-x         |
|5                    |top-left-y         |
|6                    |width              |
|7                    |height             |
|...                  | ...               |
|(COUNT - 1) x 4      |top-left-x         |
|(COUNT - 1) x 4 + 1  |top-left-y         |
|(COUNT - 1) x 4 + 2  |width              |
|(COUNT - 1) x 4 + 3  |height             |



# Models

* [Model source](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master)
* [ONNX pre-trained model](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master/models/)

# Tensor Decoders
|Framework | Links |
|---       |---    |
|GStreamer | [perm](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/blob/main/subprojects/gst-plugins-bad/gst/tensordecoders/gstfacedetectortensordecoder.c) |

