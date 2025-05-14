# Classification

- tensor-group: no
- use-cases
    - generic

# Description

Scalar expressing a count.

## Count Tensor

- tensor-shape: 1 x 1
- tensor-datatype: float32, uint8
- tensor-id: generic-variant-1-count

## Known Aliases
* num_detections:0

### Encoding
Count value stored in the tensor.

Memory layout of tensor data:

|Address| Symbol        |  Value          | Comment                 |
|---    |---            |---              |---                      |
| 0x0   | tensor-start  |  count          | complete tensor data    | 
| 0x4   | tensor-ended  |  -              | -                       | 
