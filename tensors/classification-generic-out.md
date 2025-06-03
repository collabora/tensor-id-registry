# Classification

- tensor-group: no
- use-cases
    - classification

# Description

Vector of relative confidence levels.

## Classification Tensor

- tensor-shape: 1 x n
- tensor-datatype: float32, uint8
- tensor-id: classification-generic-out

## Known Aliases
* output

### Encoding
Vector of relative confidence levels for each class.

Memory layout of tensor data:

|Index  |  Value                             |
|---    |---                                 |
| 0     |  first class confidence level      |
| 1     |  second class confidence level     | 
| ...   |  ...                               |
| n -1  |  last class confidence level       |

# External References

* [MobileNets Paper](https://arxiv.org/pdf/1704.04861)

# Models

* [ONNX model](https://gitlab.collabora.com/gstreamer/onnx-models/-/blob/master/models/mobilenetv2-10.onnx)

# Tensor Decoders
|Framework | Links |
|---       |---    |
|pytorch | [HF](https://huggingface.co/docs/transformers/en/model_doc/mobilenet_v2#transformers.MobileNetV2ForImageClassification.forward.example) |
