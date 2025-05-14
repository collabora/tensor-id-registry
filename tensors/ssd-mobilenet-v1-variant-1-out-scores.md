# Classification

- tensor-group: no
- layer-type: output
- use-case: object-detection
- part-of-tensor-groups:
    - [ssd-mobilenet-v1-variant-1-out](/tensor-groups/ssd-mobilenet-v1-variant-1-out.md)

# Description

Confidence levels of [classes](/tensors/ssd-mobilenet-v1-variant-1-out-classes.md) associated [object location](/tensors/ssd-mobilenet-v1-variant-1-out-boxes.md)

## Scores Tensor

* tensor-shape: 1 x [count]
* tensor-datatype: float32, uint8
* tensor-id: out-ssd-mobilenet-v1-scores-variant-1

### Known Aliases
* detection_scores:0

### Encoding
Number of detections corresponding to the value stored in the tensor. A higher score means the model is more certain of the accuracy of the detection.

Memory layout of tensor data:

|Index                            | Symbol                          |Value                                | Comment                 |
|---                              |---                              |---                                  |---                      |
| -                               | -                               | -                                   | -                       |
|0                                | tensor-start                    | det-1-objet-class-score             | tensor-data, start      |
|1 i                              | tensor-continue                 | det-2-objet-class-score             | tensor-data, continue   |
|...                              | ...                             | ...                                 | tensor-data, continue   |
|([count] - 1)                    | tensor-end                      | det-[count]-objet-class-score       | tensor-data, end        |
|[count]                          | -                               | -                                   | -                       |

# External References

* [MobileNets Paper](https://arxiv.org/pdf/1704.04861)

# Models

* [ONNX model trained on COCO dataset](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/acc119dd795be5e8c756457dc04507a5d9b8e768/models/ssd_mobilenet_v1_coco.onnx)

# Tensor Decoders
|Framework | Links |
|---       |---    |
|GStreamer | [perm](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/blob/c206ddd9308a3ce529e0d8957b7c165b3a15c932/subprojects/gst-plugins-bad/gst/tensordecoders/gstssdobjectdetector.c#L36-39) \| [latest](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/blob/main/subprojects/gst-plugins-bad/gst/tensordecoders/gstssdobjectdetector.c?ref_type=heads#L36-39) |


[count]: /tensors/generic-variant-1-out-count.md
