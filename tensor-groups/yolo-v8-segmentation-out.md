# YOLOv8 Segmentation

- tensor-group: yes
- layer-type: output
- use-case: instance-segmentation

## Description

Variant of YOLOv8 segmentation model outputs. This tensor group outputs two tensors 
that work together to provide instance segmentation results. The first tensor contains
detection predictions with mask coefficients, while the second tensor contains prototype
masks that are combined with the coefficients to generate final segmentation masks.

YOLOv8 Segmentation Output Tensors:
| Name                    | Shape                    | Description |
|---                     |---                       |---          |
| [detections] | 1 x 116 x 21504         | Detection results with mask coefficients |
| [protos]      | 1 x 32 x 32 x 256       | Prototype mask patterns |

## Tensor Decoding Logic

```
# Apply NMS to get valid detections
valid_detections = non_max_suppression(detections[0], conf_threshold, iou_threshold)

Foreach i in valid_detections:
    # Extract bounding box coordinates
    X = detections[0, 0, i]
    Y = detections[0, 1, i] 
    W = detections[0, 2, i]
    H = detections[0, 3, i]
    
    # Extract class probabilities and find best class
    class_scores = detections[0, 4:84, i]
    C = argmax(class_scores)
    S = max(class_scores)
    
    # Extract mask coefficients
    mask_coeffs = detections[0, 84:116, i]              # 32 coefficients
    
    # Generate segmentation mask using prototype masks
    Foreach pixel (y, x) in mask_region:
        pixel_index = y * 32 + x
        mask_value = 0.0
        
        # Linear combination of prototypes
        Foreach k in 0:255:                             # 256 prototypes
            prototype_value = protos[0, y, x, k]
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

## Models

* [YOLOv8s-seg ONNX trained on COCO](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/yolov8s-seg.onnx)


[detections]: /tensors/yolo-v8-segmentation-out-detections.md
[protos]: /tensors/yolo-v8-segmentation-out-protos.md
