# Classification

- tensor-group: no
- layer-type: output
- use-case: object-detection
- part-of-tensor-groups:
    - [ssd-mobilenet-v1-variant-1-out](/tensor-groups/ssd-mobilenet-v1-variant-1-out.md)

# Description

Classes of object associated with [object location](/tensors/ssd-mobilenet-v1-variant-1-out-boxes.md)

## Classes Tensor

* tensor-shape: 1 x [count]
* tensor-datatype: float32, uint8

### Known Aliases
* detection_classes:0

### Encoding

The classes refer to the class-id that identify the object classes in
the training data'

Scheme:
|det-1          | det-2         | ... | det-[count]  |
|---            |---            |---  |---              |
| object-class  | object-class  | ... | object-class    |


Memory layout of tensor data:

|Index         |Value                      | Comment                        |
|---             |---                        |---                             |
| -              | -                         | -                              |
|0               | det-1-object-class         | tensor-start, tensor-data      |
|1               | det-2-object-class         | tensor-continue, tensor-data   |
|...             | ...                       | tensor-continue, tensor-data   |
|[count] - 1     | det-[count]-object-class   | tensor-continue, tensor-data   | 
|([count])       | -                         | -                              |

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
