# Classification

- tensor-group: yes
- layer-type: output
- use-case: facial-landmark-detection

# Description

PIPNet outputs five tensors that must be decoded together:
1. select one coarse cell per landmark from [cls_map];
2. refine landmark coordinates with [offset_x]/[offset_y];
3. refine again with neighbor-assisted [nb_x]/[nb_y] using meanface reverse indices.

## PIPNet Output Tensors

|Name|Shape|Description|
|---|---|---|
|[cls_map]|BATCH_SIZE x NUM_LMS x FEAT_H x FEAT_W|Landmark classification heatmaps (argmax cell selection)|
|[offset_x]|BATCH_SIZE x NUM_LMS x FEAT_H x FEAT_W|Local x-offset for each landmark/cell|
|[offset_y]|BATCH_SIZE x NUM_LMS x FEAT_H x FEAT_W|Local y-offset for each landmark/cell|
|[nb_x]|BATCH_SIZE x (NUM_LMS x NUM_NB) x FEAT_H x FEAT_W|Neighbor-assisted x-offsets|
|[nb_y]|BATCH_SIZE x (NUM_LMS x NUM_NB) x FEAT_H x FEAT_W|Neighbor-assisted y-offsets|

Common settings:
- `NUM_NB = 10` in the original PIPNet experiments and in `yakhyo/pipnet-onnx`.
- For input 256x256 and `net_stride=32`, `FEAT_H = FEAT_W = 8`.

Verified release examples:
- `pipnet_r18_300w_celeba_68.onnx`: `NUM_LMS=68`, `nb_x/nb_y channels=680`.
- `pipnet_r18_wflw_98.onnx`: `NUM_LMS=98`, `nb_x/nb_y channels=980`.

## Tensor Decoding Logic

```
# Inputs:
# cls_map:  [1, NUM_LMS, FEAT_H, FEAT_W]
# offset_x: [1, NUM_LMS, FEAT_H, FEAT_W]
# offset_y: [1, NUM_LMS, FEAT_H, FEAT_W]
# nb_x:     [1, NUM_LMS*NUM_NB, FEAT_H, FEAT_W]
# nb_y:     [1, NUM_LMS*NUM_NB, FEAT_H, FEAT_W]
#
# Precomputed for target landmark schema (68/98/19):
# reverse_index1, reverse_index2, max_len

for landmark l in 0..NUM_LMS-1:
    flat_idx = argmax(cls_map[0, l, :, :])
    row = flat_idx // FEAT_W
    col = flat_idx % FEAT_W

    # direct prediction
    px[l] = (col + offset_x[0, l, row, col]) / FEAT_W
    py[l] = (row + offset_y[0, l, row, col]) / FEAT_H

    # neighbor predictions emitted by landmark l
    for k in 0..NUM_NB-1:
        ch = l*NUM_NB + k
        nb_px[l, k] = (col + nb_x[0, ch, row, col]) / FEAT_W
        nb_py[l, k] = (row + nb_y[0, ch, row, col]) / FEAT_H

# reverse gather: collect predictions about landmark i made by its neighbors
for landmark i in 0..NUM_LMS-1:
    gathered_x = nb_px[reverse_index1[i, :], reverse_index2[i, :]]
    gathered_y = nb_py[reverse_index1[i, :], reverse_index2[i, :]]

    merged_x[i] = mean(concat(px[i], gathered_x))
    merged_y[i] = mean(concat(py[i], gathered_y))

landmarks = stack(merged_x, merged_y)  # normalized to cropped-face coordinates
```

## Major Variants

All major PIPNet variants keep this same 5-tensor output structure; they differ in
`NUM_LMS`, dataset/supervision setup, and backbone.

|Variant family|Typical NUM_LMS|Backbones seen in public repos|Example source|
|---|---|---|---|
|300W / 300W+CelebA|68|ResNet-18/50/101|[PIPNet experiments: data_300W](https://github.com/jhb86253817/PIPNet/tree/master/experiments/data_300W), [PIPNet GSSL: data_300W_CELEBA](https://github.com/jhb86253817/PIPNet/tree/master/experiments/data_300W_CELEBA)|
|WFLW|98|ResNet-18/50/101|[PIPNet experiments: WFLW](https://github.com/jhb86253817/PIPNet/tree/master/experiments/WFLW)|
|AFLW|19|ResNet-18/50/101|[PIPNet experiments: AFLW](https://github.com/jhb86253817/PIPNet/tree/master/experiments/AFLW)|
|Backbone ports|68/98/19 (config-dependent)|MobileNetV2, MobileNetV3|[PIPNet network definitions](https://github.com/jhb86253817/PIPNet/blob/master/lib/networks.py)|

# External References

* [Pixel-in-Pixel Net: Towards Efficient Facial Landmark Detection in the Wild (arXiv)](https://arxiv.org/abs/2003.03771)
* [Pixel-in-Pixel Net: Towards Efficient Facial Landmark Detection in the Wild (IJCV)](https://link.springer.com/article/10.1007/s11263-021-01521-4)
* [Original PIPNet implementation](https://github.com/jhb86253817/PIPNet)
* [PIPNet ONNX exports and runtime decoder](https://github.com/yakhyo/pipnet-onnx)

# Models

* [PIPNet R18 300W+CelebA-68 ONNX](https://github.com/yakhyo/pipnet-onnx/releases/download/weights/pipnet_r18_300w_celeba_68.onnx)
* [PIPNet R18 WFLW-98 ONNX](https://github.com/yakhyo/pipnet-onnx/releases/download/weights/pipnet_r18_wflw_98.onnx)
* [Original PIPNet checkpoints (multiple datasets/backbones)](https://drive.google.com/drive/folders/17OwDgJUfuc5_ymQ3QruD8pUnh5zHreP2?usp=sharing)

# Tensor Decoders
|Framework|Links|
|---|---|
|Python (ONNX Runtime)|[yakhyo/pipnet-onnx decoder](https://github.com/yakhyo/pipnet-onnx/blob/main/model/pipnet_onnx.py)|
|PyTorch|[jhb86253817/PIPNet forward + decode utilities](https://github.com/jhb86253817/PIPNet/blob/master/lib/functions.py)|
|C++ (ONNX Runtime)|[lite.ai.toolkit PIPNet98](https://github.com/DefTruth/lite.ai.toolkit/blob/main/lite/ort/cv/pipnet98.cpp)|

[cls_map]: /tensors/pipnet-out-cls-map.md
[offset_x]: /tensors/pipnet-out-offset-x.md
[offset_y]: /tensors/pipnet-out-offset-y.md
[nb_x]: /tensors/pipnet-out-neighbor-offset-x.md
[nb_y]: /tensors/pipnet-out-neighbor-offset-y.md
