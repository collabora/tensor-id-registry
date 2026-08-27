# Classification

- tensor-group: no
- layer-type: output
- use-case: image-to-image

# Description

Generic RGB image tensor in interleaved, channel-last order. This encoding is
produced by image-to-image models whose output is itself a picture: low-light
image enhancement, denoising, deblurring, super-resolution, style transfer,
and similar tasks.

This is a generic encoding with no single originating architecture, in the
same spirit as [classification-generic-out](/tensors/classification-generic-out.md).
Models emitting an RGB image in this layout should reuse this tensor-id rather
than registering a model-specific duplicate. The reference model documented
below is Zero-DCE (Zero-Reference Deep Curve Estimation), which estimates
pixel-wise higher-order curves and applies them iteratively to the input image
to produce an enhanced image at this output.

## Image Tensor

- tensor-shape: HEIGHT x WIDTH x 3
- tensor-datatype: float32
- tensor-id: image-rgb-hwc-out
- memory-layout: HWC (row-major, channel-last)

Where:
 * HEIGHT is the input image height of the current inference
 * WIDTH is the input image width of the current inference

HEIGHT and WIDTH are properties of the model instance, not of this encoding.
A fully convolutional model accepts any resolution, while an exported artifact
may pin the shape: the original Zero-DCE PyTorch network uses stride-1
convolutions with no pooling and runs at any resolution, whereas the reference
TF-Lite export is converted from a Keras model and is pinned to a single
resolution. Zero-DCE's network also matches the output HEIGHT and WIDTH with the
input tensor resolution.

## Known Aliases
* Identity
* enhanced_image
* output_image

### Encoding

Scheme: (R: red channel, G: green channel, B: blue channel)

Each pixel is three consecutive values in row-major pixel order (top-left to
bottom-right).

Memory layout of tensor data:

|Index                              | Symbol | Value                    | Comment                                    |
|---                                 |---     |---                       |---                                         |
| -                                  | -      | -                        | -                                          |
| 0                                  | R      | pixel-0-0-red            | tensor-start, pixel-0-0-tensor-data        |
| 1                                  | G      | pixel-0-0-green          | tensor-continue, pixel-0-0-tensor-data     |
| 2                                  | B      | pixel-0-0-blue           | tensor-continue, pixel-0-0-tensor-data     |
| 3                                  | R      | pixel-0-1-red            | tensor-continue, pixel-0-1-tensor-data     |
| ...                                | ...    | ...                      | ...                                        |
| (WIDTH × 3) - 3                    | R      | pixel-0-(WIDTH-1)-red    | tensor-continue, last pixel of row 0       |
| (WIDTH × 3) - 2                    | G      | pixel-0-(WIDTH-1)-green  | tensor-continue, last pixel of row 0       |
| (WIDTH × 3) - 1                    | B      | pixel-0-(WIDTH-1)-blue   | tensor-continue, last pixel of row 0       |
| ...                                | ...    | ...                      | ...                                        |
| (HEIGHT × WIDTH × 3) - 3           | R      | pixel-(HEIGHT-1)-(WIDTH-1)-red   | tensor-continue, last pixel of last row |
| (HEIGHT × WIDTH × 3) - 2           | G      | pixel-(HEIGHT-1)-(WIDTH-1)-green | tensor-continue, last pixel of last row |
| (HEIGHT × WIDTH × 3) - 1           | B      | pixel-(HEIGHT-1)-(WIDTH-1)-blue  | tensor-end, last pixel of last row      |

### Related Encodings

Only the interleaved channel-last RGB layout is covered by this tensor-id.
Layouts that change the byte offset of a pixel component are distinct
encodings and need their own tensor-id.

The original Zero-DCE PyTorch implementation and Zero-DCE++ both output the
planar CHW layout, so they fall under that encoding rather than this
tensor-id. Their forward passes also expose the internal curve maps alongside
the image; those curve tensors are not covered here.

# External References

* [Zero-Reference Deep Curve Estimation for Low-Light Image Enhancement (CVPR 2020 paper)](https://arxiv.org/abs/2001.06826)
* [Official Zero-DCE implementation (Li-Chongyi/Zero-DCE)](https://github.com/Li-Chongyi/Zero-DCE)
* [Zero-DCE TF-Lite conversion (sayannath/Zero-DCE-TFLite)](https://github.com/sayannath/Zero-DCE-TFLite)
* [Zero-DCE++ implementation (Li-Chongyi/Zero-DCE_extension)](https://github.com/Li-Chongyi/Zero-DCE_extension)
* [Learning to Enhance Low-Light Image via Zero-Reference Deep Curve Estimation (Zero-DCE++ paper)](https://arxiv.org/abs/2103.00860)

# Models

* [TF-Lite Zero-DCE model (Kaggle Models)](https://www.kaggle.com/models/sayannath235/zero-dce)
* [Original PyTorch weights (Epoch99.pth)](https://github.com/Li-Chongyi/Zero-DCE/blob/master/Zero-DCE_code/snapshots/Epoch99.pth)

# Tensor Decoders

|Framework | Links |
|---       |---    |
|pytorch | [lowlight_test.py](https://github.com/Li-Chongyi/Zero-DCE/blob/master/Zero-DCE_code/lowlight_test.py) |
|tensorflow | [Zero-DCE Keras example](https://keras.io/examples/vision/zero_dce/) |
|tflite | [ZERO_DCE_TFLite.ipynb](https://github.com/sayannath/Zero-DCE-TFLite/blob/main/src/ZERO_DCE_TFLite.ipynb) |
