# YOLOv8 Segmentation (Normalized)

- tensor-group: yes
- layer-type: output
- use-case: instance-segmentation

## Description

Variant of YOLOv8 segmentation model outputs for models exported with older Ultralytics
versions that do not embed stride multiplication in the ONNX detect head.

This tensor group has the same structure as [yolo-v8-segmentation-out](/tensor-groups/yolo-v8-segmentation-out.md)
except that bounding box coordinates (X, Y, W, H) in the detections tensor are **normalized to [0, 1]**
relative to the input image dimensions, rather than expressed in pixel space.

YOLOv8 Segmentation Output Tensors:
| Name          | Shape                    | Description |
|---            |---                       |---          |
| [detections]  | 1 x 116 x NUM_CANDIDATES | Detection results with mask coefficients (normalized bbox) |
| [protos]      | 1 x 32 x PROTO_H x PROTO_W | Prototype mask patterns |

## Tensor Decoding Logic

```
# Apply NMS to get valid detections
valid_detections = non_max_suppression(detections[0], conf_threshold, iou_threshold)

Foreach i in valid_detections:
    # Extract normalized bounding box coordinates and scale to pixels
    X = detections[0, 0, i] * image_width
    Y = detections[0, 1, i] * image_height
    W = detections[0, 2, i] * image_width
    H = detections[0, 3, i] * image_height

    # Extract class probabilities and find best class
    class_scores = detections[0, 4:(4 + NUM_CLASSES), i]
    C = argmax(class_scores)
    S = max(class_scores)

    # Extract mask coefficients
    mask_coeffs = detections[0, (4 + NUM_CLASSES):(4 + NUM_CLASSES + NUM_MASKS), i]  # NUM_MASKS coefficients

    # Generate segmentation mask using prototype masks
    Foreach pixel (y, x) in mask_region:
        pixel_index = y * PROTO_W + x
        mask_value = 0.0

        # Linear combination of prototypes
        Foreach k in 0:(NUM_MASKS-1):                     # NUM_MASKS prototypes
            prototype_value = protos[0, k, y, x]
            mask_value += mask_coeffs[k] * prototype_value

        # Apply sigmoid and threshold
        final_mask[y, x] = sigmoid(mask_value) > 0.5 ? 1 : 0

    # Scale and crop mask to bounding box
    segmentation_mask = scale_and_crop(final_mask, X, Y, W, H, image_size)

    # Store result
    results[i] = [X, Y, W, H, C, S, segmentation_mask]
```

## External References

* [YOLOv8 Paper](https://arxiv.org/abs/2305.09972)
* [Ultralytics YOLOv8 Documentation](https://docs.ultralytics.com/models/yolov8/)
* [Ultralytics ONNX export source](https://github.com/ultralytics/ultralytics/blob/main/ultralytics/engine/exporter.py)

## Models

* [YOLOv8s-seg (old Ultralytics export)](https://github.com/ultralytics/assets/releases)

## Tensor Decoders

|Framework  | Links |
|---        |---    |
|GStreamer  | [yolov8tensordec](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/tree/main/subprojects/gst-plugins-bad/gst/tensordecoders) |

[detections]: /tensors/yolo-v8-segmentation-out-detections-normalized.md
[protos]: /tensors/yolo-v8-segmentation-out-protos.md
