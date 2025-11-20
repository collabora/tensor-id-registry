# Classification

- tensor-group: no
- layer-type: output
- use-case: object-detection
- part-of-tensor-groups:
    - [ssd-mobilenet-v1-variant-1-out](/tensor-groups/ssd-mobilenet-v1-variant-1-out.md)
    - [ultra-lightweight-face-detection-rfb-320-v1-variant-1-out](/tensor-groups/ultra-lightweight-face-detection-rfb-320-v1-variant-1-out.md)


# Description
Location of objects detected

## Boxes Tensor

- tensor-shape: 1 x [count] x 4
- tensor-datatype: float32, uint8

### Known Aliases
* detection_boxes:0

### Encoding

Scheme: (top left X, top left Y, bottom right X, bottom right Y)

The coordinates are values in the [0, 1] range where [0, 0] is the top left corrner of the image, and [1, 1] is the bottom right corner. Each set of (X, Y) coordinate is the top left and bottom right corner of a possible bounding box.

|box-1 | box-1 | box-1 | box-1 | box-2 | box-2 | box-2 | box-2 | ... | [count]|[count]|[count]|[count]|
|---   |---    |---    |---    |---    |---    |---    |---    |---  |---                |---                |---                |---                |
| top-left-X | top-left-Y | bottom-right-X | bottom-right-Y | top-left-X | top-left-Y | bottom-right-X | bottom-right-Y | ...  top-left-X | top-left-Y | bottom-right-X | bottom-right-Y |

Memory layout of tensor data:

|Index                  | Symbol            |Value              | Comment                             |
|---                    |---                |---                |---                                  |
| -                     | -                 | -                 | -                                   |
|0                      |                   |top-left-x-coord            |tensor-start, box-0-tensor-data      |
|1                      |                   |top-left-y-coord            |tensor-continue, box-0-tensor-data   |
|2                      |                   |bottom-right-x-coord              |tensor-continue, box-0-tensor-data   |
|3                      |                   |bottom-right-y-coord             |tensor-continue, box-0-tensor-data   |
|4                      |                   |top-left-x-coord            |tensor-continue, box-1-tensor-data   |
|5                      |                   |top-left-y-coord            |tensor-continue, box-1-tensor-data   |
|6                      |                   |bottom-right-x-coord              |tensor-continue, box-1-tensor-data   |
|7                      |                   |bottom-right-y-coord             |tensor-continue, box-1-tensor-data   |
|...                    | ...               | ...               | ...                                 |
|([count] - 1) x 4      |                   |top-left-x-coord            |tensor-continue, [count]-tensor-data |
|([count] - 1) x 4 + 1  |                   |top-left-y-coord            |tensor-continue, [count]-tensor-data |
|([count] - 1) x 4 + 2  |                   |bottom-right-x-coord              |tensor-continue, [count]-tensor-data |
|([count] - 1) x 4 + 3  |                   |bottom-right-y-coord             |tensor-continue, [count]-tensor-data |
| -                     | -                 | -                 | -                                   |

# External References

* [MobileNets Paper](https://arxiv.org/pdf/1704.04861)
* [ONNX model trained on COCO dataset](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/acc119dd795be5e8c756457dc04507a5d9b8e768/models/ssd_mobilenet_v1_coco.onnx)

# Models

* [ONNX model trained on COCO dataset](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/acc119dd795be5e8c756457dc04507a5d9b8e768/models/ssd_mobilenet_v1_coco.onnx)

# Tensor Decoders
|Framework | Links |
|---       |---    |
|GStreamer | [perm](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/blob/c206ddd9308a3ce529e0d8957b7c165b3a15c932/subprojects/gst-plugins-bad/gst/tensordecoders/gstssdobjectdetector.c#L36-39) \| [latest](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/blob/main/subprojects/gst-plugins-bad/gst/tensordecoders/gstssdobjectdetector.c?ref_type=heads#L36-39) |


[count]: /tensors/generic-variant-1-out-count.md
