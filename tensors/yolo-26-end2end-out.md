# Classification

- tensor-group: no
- layer-type: output
- use-case: object-detection

# Description

End-to-end NMS-free detection output from YOLO26 object detection models. This tensor is
produced by YOLO26's **one-to-one head**, which is the only head present at inference time.
Unlike [YOLO v8](/tensors/yolo-v8-out.md), no Non-Maximum Suppression post-processing is
required: the model internally selects the single best prediction per object during training
and outputs a fixed-size list of finalised detections.

The key architectural differences that produce this distinct tensor format are:
- `end2end: True` — the one-to-many training head is discarded at export; only the one-to-one head remains.
- `reg_max: 1` — Distribution Focal Loss (DFL) is replaced by an identity operation, enabling direct coordinate regression.

## Detection Predictions Tensor

- tensor-shape: BATCH_SIZE x MAX_DET x 6
- tensor-datatype: float32
- tensor-id: yolo-26-end2end-out
- memory-layout: row-major order

Where:
 * BATCH_SIZE is the size of the batch
 * MAX_DET is the maximum number of detections per image (default: 300)

### Known Aliases
* output0

### Encoding

Scheme: (X1: left edge, Y1: top edge, X2: right edge, Y2: bottom edge, S: confidence score, C: class index)

Each detection is a contiguous row of 6 float32 values. Bounding box coordinates are in
absolute pixel space relative to the model input resolution (e.g. 640×640). Rows beyond the
number of actual detections in the scene are present but carry low confidence scores and
should be filtered with a confidence threshold.

| Field 0 | Field 1 | Field 2 | Field 3 | Field 4    | Field 5      |
|---------|---------|---------|---------|------------|--------------|
| X1      | Y1      | X2      | Y2      | Score      | Class Index  |

Memory layout of tensor data:

| Index               | Symbol | Value                  | Comment                                              |
|---------------------|--------|------------------------|------------------------------------------------------|
| 0                   | X1     | det-0-x1               | tensor-start, det-0-start, absolute pixels           |
| 1                   | Y1     | det-0-y1               | absolute pixels                                      |
| 2                   | X2     | det-0-x2               | absolute pixels                                      |
| 3                   | Y2     | det-0-y2               | absolute pixels                                      |
| 4                   | S      | det-0-confidence       | sigmoid-activated max class probability, range [0,1] |
| 5                   | C      | det-0-class-index      | integer class index stored as float32, det-0-end     |
| ...                 | ...    | ...                    | ...                                                  |
| MAX_DET × 6 - 6     | X1     | det-C-x1               | det-C-start, absolute pixels                         |
| MAX_DET × 6 - 5     | Y1     | det-C-y1               | absolute pixels                                      |
| MAX_DET × 6 - 4     | X2     | det-C-x2               | absolute pixels                                      |
| MAX_DET × 6 - 3     | Y2     | det-C-y2               | absolute pixels                                      |
| MAX_DET × 6 - 2     | S      | det-C-confidence       | sigmoid-activated max class probability, range [0,1] |
| MAX_DET × 6 - 1     | C      | det-C-class-index      | integer class index stored as float32, tensor-end    |

# External References

* [YOLO26 Architecture Overview (arXiv)](https://arxiv.org/abs/2602.14582)
* [Ultralytics YOLO26 Documentation](https://docs.ultralytics.com/models/yolo26/)
* [Ultralytics head.py — Detect class](https://github.com/ultralytics/ultralytics/blob/main/ultralytics/nn/modules/head.py)
* [Ultralytics yolo26.yaml — model config](https://github.com/ultralytics/ultralytics/blob/main/ultralytics/cfg/models/26/yolo26.yaml)

# Models

* [YOLO26n pretrained on COCO (Ultralytics)](https://github.com/ultralytics/assets/releases/download/v8.4.0/yolo26n.pt)

# Tensor Decoders

| Framework | Links |
|-----------|-------|
| Ultralytics (Python) | [ultralytics](https://github.com/ultralytics/ultralytics) |
