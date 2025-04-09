# Classification
- tensor-group: yes
- layer-type: output
- use-case: object-detection

# Description

Variant of ssd-mobilenet-v1 outputs. This tensor group contains four 
tensors that together describe the predictions ssd-mobilenet-v1. Count tensor is 
a scalar that represents the number of object detections in the other 3 tensors.

SSD-MobileNetV1 Variant-1 Output Tensors:

| Name          |shape               |
|---            |---                 |
| [count]       |1 x 1               |
| [boxes]       |1 x [count] x 4     |
| [classes]     |1 x [count]         |
| [scores]      |1 x [count]         |

# Tensor Decoding Logic

```
Foreach i in count:
    X = boxes[i, 0]
    Y = boxes[i, 1]
    W = boxes[i, 2]
    H = boxes[i, 3]
    C = classes[i]
    S = scores[i]
    detections[i] = [X, Y, W, H, C, S]
```

# External References

* [MobileNets Paper](https://arxiv.org/pdf/1704.04861)
* [ONNX model trained on COCO dataset](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/acc119dd795be5e8c756457dc04507a5d9b8e768/models/ssd_mobilenet_v1_coco.onnx)

# Models

* [ONNX model trained on COCO dataset](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/acc119dd795be5e8c756457dc04507a5d9b8e768/models/ssd_mobilenet_v1_coco.onnx)

# Tensor Decoders
|Framework | Links |
|---       |---    |
|GStreamer | [perm](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/blob/c206ddd9308a3ce529e0d8957b7c165b3a15c932/subprojects/gst-plugins-bad/gst/tensordecoders/gstssdobjectdetector.c#L36-39) \| [latest](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/blob/main/subprojects/gst-plugins-bad/gst/tensordecoders/gstssdobjectdetector.c?ref_type=heads#L36-39) |


[count]: /tensor-groups/generic-variant-1-out-count.md
[boxes]: /tensors/ssd-mobilenet-v1-variant-1-out-boxes.md
[classes]: /tensors/ssd-mobilenet-v1-variant-1-out-classes.md
[scores]: /tensors/ssd-mobilenet-v1-variant-1-out-scores.md

