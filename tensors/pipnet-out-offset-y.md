# Classification

- tensor-group: no
- layer-type: output
- use-case: facial-landmark-detection
- part-of-tensor-groups:
    - [pipnet-out](/tensor-groups/pipnet-out.md)

# Description

Per-landmark local Y offsets predicted at each feature-map cell. During decoding,
the selected cell index from [cls_map] is refined with this offset.

## Local Y Offset Tensor

- tensor-shape: BATCH_SIZE x NUM_LMS x FEAT_H x FEAT_W
- tensor-datatype: float32
- memory-layout: NCHW row-major (C-order)

Where:
- `NUM_LMS` follows the trained landmark schema (68, 98, 19, ...).
- `FEAT_H x FEAT_W` is the output grid size (8x8 for 256x256 input with stride 32).

Examples from `yakhyo/pipnet-onnx` release models:
- `pipnet_r18_300w_celeba_68.onnx`: 1 x 68 x 8 x 8
- `pipnet_r18_wflw_98.onnx`: 1 x 98 x 8 x 8

### Known Aliases
* offset_y
* outputs_y

### Encoding

For landmark `l`, if `argmax(cls_map[l])` is at cell `(row_l, col_l)`, the normalized
Y estimate before neighbor-averaging is:

`y_l = (row_l + offset_y[l, row_l, col_l]) / FEAT_H`

Let `SPATIAL_SIZE = FEAT_H x FEAT_W`.

Memory layout of tensor data for `BATCH_SIZE=1`:

|Index|Symbol|Value|Comment|
|---|---|---|---|
|0|DY(0,0,0)|landmark-0 cell-(0,0) y-offset|tensor-start, landmark-0 plane-start|
|...|...|...|...|
|SPATIAL_SIZE x 1 - 1|DY(0,FEAT_H-1,FEAT_W-1)|landmark-0 last-cell y-offset|landmark-0 plane-end|
|SPATIAL_SIZE x 1|DY(1,0,0)|landmark-1 cell-(0,0) y-offset|landmark-1 plane-start|
|...|...|...|...|
|SPATIAL_SIZE x NUM_LMS - 1|DY(NUM_LMS-1,FEAT_H-1,FEAT_W-1)|last landmark last-cell y-offset|tensor-end|

# External References

* [Pixel-in-Pixel Net: Towards Efficient Facial Landmark Detection in the Wild (arXiv)](https://arxiv.org/abs/2003.03771)
* [Pixel-in-Pixel Net: Towards Efficient Facial Landmark Detection in the Wild (IJCV)](https://link.springer.com/article/10.1007/s11263-021-01521-4)
* [Original PIPNet implementation](https://github.com/jhb86253817/PIPNet)

# Models

* [PIPNet R18 300W+CelebA-68 ONNX](https://github.com/yakhyo/pipnet-onnx/releases/download/weights/pipnet_r18_300w_celeba_68.onnx)
* [PIPNet R18 WFLW-98 ONNX](https://github.com/yakhyo/pipnet-onnx/releases/download/weights/pipnet_r18_wflw_98.onnx)

# Tensor Decoders
|Framework|Links|
|---|---|
|Python (ONNX Runtime)|[yakhyo/pipnet-onnx decoder](https://github.com/yakhyo/pipnet-onnx/blob/main/model/pipnet_onnx.py)|
|PyTorch|[jhb86253817/PIPNet decoding path](https://github.com/jhb86253817/PIPNet/blob/master/lib/functions.py#L173-L214)|

[cls_map]: /tensors/pipnet-out-cls-map.md
