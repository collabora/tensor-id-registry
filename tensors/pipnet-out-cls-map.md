# Classification

- tensor-group: no
- layer-type: output
- use-case: facial-landmark-detection
- part-of-tensor-groups:
    - [pipnet-out](/tensor-groups/pipnet-out.md)

# Description

Per-landmark classification heatmaps used by PIPNet to select one coarse grid cell
for each landmark before sub-pixel offset refinement.

## Classification Map Tensor

- tensor-shape: BATCH_SIZE x NUM_LMS x FEAT_H x FEAT_W
- tensor-datatype: float32
- memory-layout: NCHW row-major (C-order)

Where:
- BATCH_SIZE is usually 1 for inference.
- NUM_LMS is the landmark count (68 for 300W variants, 98 for WFLW variants, 19 for AFLW variants).
- FEAT_H and FEAT_W are feature-map dimensions. For 256x256 input with net_stride=32, FEAT_H=FEAT_W=8.

Examples from `yakhyo/pipnet-onnx` release models:
- `pipnet_r18_300w_celeba_68.onnx`: 1 x 68 x 8 x 8
- `pipnet_r18_wflw_98.onnx`: 1 x 98 x 8 x 8

### Known Aliases
* cls_map
* outputs_cls

### Encoding

For each landmark channel, the highest-scoring spatial location (`argmax`) selects the
coarse grid cell where that landmark is located.

Let `SPATIAL_SIZE = FEAT_H x FEAT_W`.

Memory layout of tensor data for `BATCH_SIZE=1`:

|Index|Symbol|Value|Comment|
|---|---|---|---|
|0|CLS(0,0,0)|landmark-0 cell-(0,0) score|tensor-start, landmark-0 plane-start|
|...|...|...|...|
|SPATIAL_SIZE x 1 - 1|CLS(0,FEAT_H-1,FEAT_W-1)|landmark-0 last-cell score|landmark-0 plane-end|
|SPATIAL_SIZE x 1|CLS(1,0,0)|landmark-1 cell-(0,0) score|landmark-1 plane-start|
|...|...|...|...|
|SPATIAL_SIZE x NUM_LMS - 1|CLS(NUM_LMS-1,FEAT_H-1,FEAT_W-1)|last landmark last-cell score|tensor-end|

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
