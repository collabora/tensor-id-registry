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

# Tensor Decoding Logic

```
Foreach i in count:
    X = boxes_processed(i, 0)
    Y = boxes_processed(i, 1)
    W = boxes_processed(i, 2)
    H = boxes_processed(i, 3)
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

