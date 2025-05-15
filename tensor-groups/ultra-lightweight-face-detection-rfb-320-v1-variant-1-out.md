# Classification
- tensor-group: yes
- layer-type: output
- use-case: face-detection

# Description

Variant of ultra-lightweight-face-detection-rfb-320-v1 outputs. This tensor group contains two 
tensors that together describe the predictions of ultra-lightweight-face-detection-rfb-320-v1. Scores tensor
represents the confidence levels for foreground (face) or background (no face) for each bounding box.

ultra-lightweight-face-detection-rfb-320-v1 Variant-1 Output Tensors:

| Name          |shape                                       |
|---            |---                                         |
| [boxes]       |1 x Num of Bounding Box predictions x 4     |
| [scores]      |1 x Num of Bounding Box predictions x 2     |

# Tensor Decoding Logic

```
Foreach i in Num_of_Bounding_Box_predictions:
    boxes_index = i * 4
    predictions_index = i * 2
    if scores[predictions_index] >= score_threshold:
        selected.add(boxes[boxes_index:boxes_index+4],
            scores[predictions_index:predictions_index+1])
```

### NMS
An optional NMS can be performed to remove the overlappings

```
Foreach i in Num_of_Bounding_Box_predictions:
    :
    :
    boxes_with_face_detected = nms(selected)
```

# External References

* [Ultra Light Fast Face Detector](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB)
* [ONNX models](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master/models/onnx)

# Models

* [ONNX models](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master/models/onnx)

# Tensor Decoders
|Framework | Links |
|---       |---    |
|GStreamer | [latest](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/blame/main/subprojects/gst-plugins-bad/gst/tensordecoders/gstfacedetectortensordecoder.c?ref_type=heads#L38-42) |


[boxes]: /tensors/ssd-mobilenet-v1-variant-1-out-boxes.md
[scores]: /tensors/ultra-lightweight-face-detection-rfb-320-v1-variant-1-out-scores.md
