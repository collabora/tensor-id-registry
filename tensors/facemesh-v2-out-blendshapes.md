# Classification

- tensor-group: no
- layer-type: output
- use-case: face-landmarks
- part-of-tensor-groups:
  - [facemesh-v2-out](/tensor-groups/facemesh-v2-out.md)

# Description
The blendshapes tensor output of the MediaPipe FaceMesh V2 model. This contains a scalar representing the tongueOut blendshape coefficient, whose range is the interval (0, 1). This blendshape activation value may be thought of as the extent to which the tongue sticking out of the face.

## Face Landmarks Tensor

- tensor-shape: BATCH_SIZE x 1 x 1 x 1
- tensor-datatype: float32
- tensor-id: facemesh-v2-out-blendshapes
- memory-layout: row-major order

Where:
* BATCH_SIZE is the size of the batch

### Known Aliases
* Identity_2

# Encoding

Scheme: [B]

- B = blendshapes

Memory layout of tensor data:

| Address | Symbol | Value       | Comment                            |
|---------|--------|-------------|------------------------------------|
| 0x0     | P      | blendshapes | tensor-start, complete tensor data |
| 0x4     | -      | -           | tensor-end                         |

# Tensor Decoding Logic

```
for b in 0 .. BATCH_SIZE - 1:
  B = blendshapes[b, 0, 0, 0]
```

# External References

* [MediaPipe FaceMesh V2 Model Card](https://storage.googleapis.com/mediapipe-assets/Model%20Card%20MediaPipe%20Face%20Mesh%20V2.pdf)
* [MediaPipe Blendshape V2 Model Card](https://storage.googleapis.com/mediapipe-assets/Model%20Card%20Blendshape%20V2.pdf)
* [MediaPipe Face landmark detection guide](https://developers.google.com/edge/mediapipe/solutions/vision/face_landmarker#task_details)
* [MediaPipe FaceMesh V1 Paper](https://arxiv.org/pdf/1907.06724)
* [MediaPipe FaceMesh V1 Docs](https://github.com/google-ai-edge/mediapipe/blob/76f1d78eb1c350f882536adfa84d248ad0c84a60/docs/solutions/face_mesh.md)
* [MediaPipe Repository](https://github.com/google-ai-edge/mediapipe)

# Models

* [Face Landmarker Model Bundle](https://developers.google.com/edge/mediapipe/solutions/vision/face_landmarker#models)
> To obtain the FaceMesh V2 tflite model, `unzip` the model bundle (the `.task` file). Upon extraction, you can find the FaceMesh V2 model under the name `face_landmarks_detector.tflite`.

# Tensor Decoders

* https://github.com/yakhyo/uniface/blob/main/uniface/landmark/facemesh.py
* https://github.com/cornpip/mediapipe_face_mesh/blob/master/src/mediapipe_face_mesh.cc
* https://github.com/ailia-ai/ailia-models/blob/master/face_recognition/facemesh_v2/facemesh_v2.py
* https://github.com/terryky/tflite_gles_app/blob/master/gl2facemesh/tflite_facemesh.cpp
* https://github.com/PINTO0309/facemesh_onnx_tensorrt/blob/main/facemesh_postprocess.py
