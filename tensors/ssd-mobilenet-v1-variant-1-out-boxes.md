# Classification

- tensor-group: no
- layer-type: output
- use-case: object-detection
- part-of-tensor-groups:
    - [ssd-mobilenet-v1-variant-1-out](/tensor-groups/ssd-mobilenet-v1-variant-1-out.md)

# Description
Location of objects detected

## Boxes Tensor

- tensor-shape: 1 x [count] x 4
- tensor-datatype: float32, uint8

### Known Aliases
* detection_boxes:0

### Encoding

Scheme: (X: box x coord, Y: box y coord, W: box width, H: box height)

|box-1 | box-1 | box-1 | box-1 | box-2 | box-2 | box-2 | box-2 | ... | [count]|[count]|[count]|[count]|
|---   |---    |---    |---    |---    |---    |---    |---    |---  |---                |---                |---                |---                |
| X | Y | W | H | X | Y | W | H |...| X | Y | W | H |

Memory layout of tensor data:

|Index                  | Symbol            |Value              | Comment                             |
|---                    |---                |---                |---                                  |
| -                     | -                 | -                 | -                                   |
|0                      |                   |x-coord            |tensor-start, box-0-tensor-data      |
|1                      |                   |y-coord            |tensor-continue, box-0-tensor-data   |
|2                      |                   |width              |tensor-continue, box-0-tensor-data   |
|3                      |                   |height             |tensor-continue, box-0-tensor-data   |
|4                      |                   |x-coord            |tensor-continue, box-1-tensor-data   |
|5                      |                   |y-coord            |tensor-continue, box-1-tensor-data   |
|6                      |                   |width              |tensor-continue, box-1-tensor-data   |
|7                      |                   |height             |tensor-continue, box-1-tensor-data   |
|...                    | ...               | ...               | ...                                 |
|([count] - 1) x 4      |                   |x-coord            |tensor-continue, [count]-tensor-data |
|([count] - 1) x 4 + 1  |                   |y-coord            |tensor-continue, [count]-tensor-data |
|([count] - 1) x 4 + 2  |                   |width              |tensor-continue, [count]-tensor-data |
|([count] - 1) x 4 + 3  |                   |height             |tensor-continue, [count]-tensor-data |
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
