# Classification

- tensor-group: yes
- layer-type: output
- use-case: face-landmarks

# Description

Output tensor group for the MediaPipe FaceMesh V1 facial landmarker model. This tensor group contains two tensors that represent facial landmark results. The first tensor contains 468 3D landmarks. The second tensor contains a logit denoting the probability that a face is present in the input image.

FaceMesh V1 Output Tensors:

| Name          | Shape                     |
|---------------|---------------------------|
| [landmarks]   | 1 x 1 x 1 x 1404          |
| [presence]    | 1 x 1 x 1 x 1             |

# Tensor Decoding Logic

```
# Extract 468 landmarks
for l = 0 .. 468:
  X_l = landmarks[0, 0, 0, l * 3]
  Y_l = landmarks[0, 0, 0, l * 3 + 1]
  Z_l = landmarks[0, 0, 0, l * 3 + 2]
  
# Extract presence logit
P_logit = presence[0, 0, 0, 0]

# Apply standard logistic function to retrieve presence probability
P = 1/(1 + exp(-P_logit))
```

# External References

* [MediaPipe FaceMesh V1 Paper](https://arxiv.org/pdf/1907.06724)
* [MediaPipe FaceMesh V1 Docs](https://github.com/google-ai-edge/mediapipe/blob/76f1d78eb1c350f882536adfa84d248ad0c84a60/docs/solutions/face_mesh.md)
* [MediaPipe FaceMesh V1 Model Card](https://drive.google.com/file/d/1QvwWNfFoweGVjsXF3DXzcrCnz-mx-Lha/preview)
* [MediaPipe Repository](https://github.com/google-ai-edge/mediapipe)
* [Facial Landmarks Mapping Image](https://storage.googleapis.com/mediapipe-assets/documentation/mediapipe_face_landmark_fullsize.png)
* [Facial Landmarks Connections (Python)](https://github.com/google-ai-edge/mediapipe/blob/76f1d78eb1c350f882536adfa84d248ad0c84a60/mediapipe/tasks/python/vision/face_landmarker.py#L109-L2869)

# Models

* [FaceMesh V1 Model (tflite)](https://github.com/google-ai-edge/mediapipe/blob/master/docs/solutions/models.md#face-mesh)

# Tensor Decoders

* https://github.com/yakhyo/uniface/blob/main/uniface/landmark/facemesh.py
* https://github.com/cornpip/mediapipe_face_mesh/blob/master/src/mediapipe_face_mesh.cc
* https://github.com/terryky/tflite_gles_app/blob/master/gl2facemesh/tflite_facemesh.cpp
* https://github.com/PINTO0309/facemesh_onnx_tensorrt/blob/main/facemesh_postprocess.py


[landmarks]: /tensors/facemesh-v1-out-landmarks.md
[presence]: /tensors/facemesh-v1-out-presence.md
