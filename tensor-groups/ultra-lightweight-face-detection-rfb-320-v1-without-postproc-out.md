# Classification
- tensor-group: yes
- layer-type: output
- use-case: face-detection

# Description

A specialized model that detects faces

Ultra-Light-Fast-Generic-Face-Detector Output Tensors:

| Name          |shape             |
|---            |---               |
| [boxes]       |1 x COUNT x 4     |
| [scores]      |1 x COUNT x 2     |

Where COUNT is a value selected at training time.

# Anchor Generation

Anchors are reference bounding boxes distributed regularly across the input image at different scales and aspect ratios. They serve as starting points for the model to predict face locations. The process below describes how anchors are generated for each feature map and min box size, ensuring coverage of the image at multiple resolutions. Each anchor is defined by its center coordinates and normalized width and height.

Anchors are generated at training time using the following parameters for input size 320×240:

- Feature maps: `[[40, 30], [20, 15], [10, 8], [5, 4]]`  (width × height)
- Min boxes: `[[10, 16, 24], [32, 48, 72], [128, 192, 256], [256, 384, 512]]`  (width × height)

Pseudocode:
```
For each (feature_map, min_boxes) in zip(feature_maps, min_boxes):
    feature_map_w, feature_map_h = feature_map
    For each min_box in min_boxes:
        If min_box == 0:
            continue
        box_w = min_box / input_width
        box_h = min_box / input_height

        For x in 0 to feature_map_w - 1:
            For y in 0 to feature_map_h - 1:
                center_x = (x + 0.5) / feature_map_w
                center_y = (y + 0.5) / feature_map_h

                Create anchor: [center_x, center_y, box_w, box_h]
```

Final anchors are clipped to [0.0, 1.0] range.

# Tensor Decoding Logic

Those constants are extracted from the training process:
- CENTER_VARIANCE = 0.1
- SIZE_VARIANCE = 0.2
- COUNT

```
Foreach i in COUNT:
    base_idx = i * 4
    delta-center-x = boxes[base_idx + 0]
    delta-center-y = boxes[base_idx + 1]
    delta-log-width = boxes[base_idx + 2]
    delta-log-height = boxes[base_idx + 3]

    X = delta-center-x * CENTER_VARIANCE * anchor-width(i) + anchor-center-x(i)
    Y = delta-center-y * CENTER_VARIANCE * anchor-height(i) + anchor-center-y(i)
    W = e^(delta-log-width * SIZE_VARIANCE) * anchor-width(i)
    H = e^(delta-log-height * SIZE_VARIANCE) * anchor-height(i)

    S = scores[i][1]
    If S > threshold:
        detection_candidates[j++] = [X, Y, W, H, S]
detections = non_max_suppression(detection_candidates)

```

Where X, Y, W and H are values between 0 and 1. The boxes have been processed
from the anchors.

# External References
* https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB

# Models
* [Model source](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master)
* [ONNX pre-trained model](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master/models/)

# Tensor Decoders
|Framework | Links |
|---       |---    |
|GStreamer | [perm](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/blob/main/subprojects/gst-plugins-bad/gst/tensordecoders/gstfacedetectortensordecoder.c) |


[boxes]: /tensors/ultra-lightweight-face-detection-rfb-320-v1-variant-1-out-boxes-without-postproc.md
[scores]: /tensors/ultra-lightweight-face-detection-rfb-320-v1-variant-1-out-scores.md

