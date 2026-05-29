# Classification

- tensor-group: no
- layer-type: output
- use-case: object-detection

# Description

Combined detection predictions containing bounding boxes and class probabilities YOLOX object detection models model.

## Detection Predictions Tensor

- tensor-shape: BATCH_SIZE x (4 + 1 + NUM_CLASSES) x NUM_CANDIDATES)
- tensor-datatype: float32
- memory-layout: row-major order

Where:
 * BATCH_SIZE is the size of the batch
 * NUM_CLASSES is the number the classes the model was train
 * NUM_CANDIDATES is the total number of candidates

### Encoding

Scheme: (X: x-coord, Y: y-coord, W: width, H: height, B: box probability, C0-Cx: class probabilities)

The tensor contains many channels with the following layout per spatial location:

|Channels 0-3  | Channel 4 | Channels 5-x |
|---           |---        |---           |
| Bounding Box | Box Prob  | Class Probs  |
| [X,Y,W,H]    | B         | [C0...Cx]    |

Memory layout of tensor data (for spatial location i):

Memory layout of tensor data:

|Index                         | Symbol            | Value                    | Comment                                    |
|---                           |---                |---                       |---                                         |
| -                            | -                 | -                        | -                                          |
|0                             | X                 | box-0-x                  | tensor-start, x                            |
|1                             | Y                 | box-0-y                  | tensor-continue, y                         |
|2                             | W                 | box-0-width              | tensor-continue, width                     |
|3                             | H                 | box-0-height             | tensor-continue, height                    |
|4                             | B                 | box-0-box-prob           | tensor-continue, box-probability           |
|5                             | C0                | box-0-class-0-prob       | tensor-continue, class-0-prob              |
|6                             | C1                | box-0-class-1-prob       | tensor-continue, class-1-prob              |
|...                           | ...               | ...                      | ...                                        |
|4 + 1 + NUM_CLASSES - 1       | Cx                | box-0-class-x-prob       | tensor-continue, class-x-prob              |
|(4 + 1 + NUM_CLASSES)         | X                 | box-1-x                  | tensor-continue, x                         |
|...                           | ...               | ...                      | ...                                        |
|NUM_CANDIDATES × (4 + 1 + NUM_CLASSES) - 1 | Cx                | box-C-class-x-prob      | tensor-end, class-x-prob     |

# External References

* [YOLOX Paper](https://arxiv.org/abs/2107.08430)
* [YOLOX Website](https://yolox.readthedocs.io/en/latest/)

