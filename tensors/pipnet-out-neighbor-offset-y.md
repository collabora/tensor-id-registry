# Classification

- tensor-group: no
- layer-type: output
- use-case: facial-landmark-detection
- part-of-tensor-groups:
    - [pipnet-out](/tensor-groups/pipnet-out.md)

# Description

Neighbor-assisted local Y offsets for PIPNet. These channels allow each landmark to
predict positions for its `NUM_NB` nearest landmarks, then merge those predictions
with each landmark's own estimate.

## Neighbor Local Y Offset Tensor

- tensor-shape: BATCH_SIZE x (NUM_LMS x NUM_NB) x FEAT_H x FEAT_W
- tensor-datatype: float32
- memory-layout: NCHW row-major (C-order)

Where:
- `NUM_NB` is the per-landmark neighbor count (10 in original PIPNet configurations).
- Channel mapping is `channel = landmark_id x NUM_NB + neighbor_slot`.

Examples from `yakhyo/pipnet-onnx` release models:
- `pipnet_r18_300w_celeba_68.onnx`: 1 x 680 x 8 x 8
- `pipnet_r18_wflw_98.onnx`: 1 x 980 x 8 x 8

### Known Aliases
* nb_y
* outputs_nb_y

### Encoding

For landmark `l`, let `(row_l, col_l)` be the argmax cell from `cls_map[l]`.
For neighbor slot `k`, channel `c = l x NUM_NB + k` predicts:

`nb_y_lk = (row_l + nb_y[c, row_l, col_l]) / FEAT_H`

These predictions are later reverse-gathered with meanface neighbor indices and
averaged with direct landmark predictions.

Let:
- `CHANNELS = NUM_LMS x NUM_NB`
- `SPATIAL_SIZE = FEAT_H x FEAT_W`

Memory layout of tensor data for `BATCH_SIZE=1`:

|Index|Symbol|Value|Comment|
|---|---|---|---|
|0|NBY(0,0,0)|channel-0 cell-(0,0) neighbor-y-offset|tensor-start, channel-0 plane-start|
|...|...|...|...|
|SPATIAL_SIZE x 1 - 1|NBY(0,FEAT_H-1,FEAT_W-1)|channel-0 last-cell neighbor-y-offset|channel-0 plane-end|
|SPATIAL_SIZE x 1|NBY(1,0,0)|channel-1 cell-(0,0) neighbor-y-offset|channel-1 plane-start|
|...|...|...|...|
|SPATIAL_SIZE x CHANNELS - 1|NBY(CHANNELS-1,FEAT_H-1,FEAT_W-1)|last channel last-cell neighbor-y-offset|tensor-end|

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
