# General
The general idea of this tensor-id registry is to provide an identifier for a 
tensor encoding that is not associated with any specific framework or 
programming language. The tensor description should be complet enough to allow
the interpretation of the tensor without additional reference. This register 
is meant to facilitate N.N. usage in a framework agnostic way. Internal details
and layers of the N.N. are not necessarily in scope, but if you believe their is value
in using this register to describe internal layer you are welcome to submit by 
accompanying your submission with and explanation of the value added. Tensor 
encoding submission should include at least one reference to the original paper
from which this encoding emerge or derive, one link to model using this encoding
 and one link to a code, publically available, able to interpret the tensor encoding.

# Tensor Id
The identifier of the tensor must be unique for a complet definition of a tensor.

# Tensor Description File
- Use markdown
- At the time of writing this, the filename of the file used to described the tensor
is used as a storage of the tensor-id.
- Filename is composed of {tensor-id}.md

## Selecting a tensor-id

Anatomy of an tensor-id

1. Only use alpha-numeric character separated by `-` 
2. Name of the first architecture where this tensor encoding has emerged
3. Append "-variant-1" term if this encoding is a derivative of the original tensor encoding. If a "variant-1" already exist, use the next variant index. Ex."variant-2". Append "-out" it the tensor is an output of the inference. Use "-in" it is an input of the inference. Use "-inout" if it's output and an input of the inference. 
4. If the this tensor is part of a tensor-group, append a term that identify the function of this tensor in the tensor-group.

Ex:

Original N.N. Architecture: "ssd-mobilenet-v1-variant-1

# Keywords

## iref
Stand for internal reference. Prefix another keywork `iref` to signify this come from a different tensor.
Create 

Example:

Tensor A:
iref: count
tensor-shape: 1x1
tensor-datatype: float32
tensor-id: tensor-a
encoding: tensor is a scalar and only contain one value that represent a count.


Tensor B:
iref: scores
tensor-shape: 1 x [iref:count](submission-guide-line.md)

### Formating
* Since iref reference should not contain any space.

## Sections

### Internal References
Reference internal to the registry. These section contain an association 
between a term used to represent a tensor that is described in a different file.
Additionnaly if contain a link to the file containing the complete descripiton 
of the tensor.

### Classification

Classification of the tensor-encoding and tensor-groups.

#### tensor-group
Indicate if the content describe a tensor-group or a tensor.


#### layer-type
Indicate if the content describe `input`, `output` or `inout` tensor.

#### use-case
In which context this tensor-encoding is used.

#### part-of-tensor-groups
List tensor-groups using this tensor, if it is not a generic tensor.

### Description
Provide a descrition sufficient to understand the information it contain in
in relation to analysis result and if it's associated to other tensor.

#### iref
Define an iref tag to allow other tensor to refer this tensor description.

#### tensor-shape
Provide the tensor dimensions

#### tensor-datatype
Provide the datatype used by this tensor

### Known Aliases
List known aliases of this tensor encoding

### Encoding

Describe any pattern used to encode analysis result in the tensor.
Provide a memory layout of the tensor encoding.


