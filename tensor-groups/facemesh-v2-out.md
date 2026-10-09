# Classification

- tensor-group: yes
- layer-type: output
- use-case: face-landmarks

# Description

Output tensor group for the MediaPipe FaceMesh V2 facial landmarker model. This tensor group contains three tensors that represent facial landmark results. The first tensor contains 478 3D landmarks. The second tensor contains a logit denoting the probability that a face is present in the input image. The third tensor contains the blendshape coefficient, in the interval (0, 1), for the tongueOut blendshape. This may be thought of as the extent to which the tongue sticking out of the face.

FaceMesh V2 Output Tensors:

| Name          | Shape                     |
|---------------|---------------------------|
| [landmarks]   | BATCH_SIZE x 1 x 1 x 1434 |
| [presence]    | BATCH_SIZE x 1 x 1 x 1    |
| [blendshapes] | BATCH_SIZE x 1            |

# Tensor Decoding Logic

```
for b = 0 .. BATCH_SIZE - 1:
  # Extract 478 landmarks
  for l = 0 .. 478:
    X_l = landmarks[b, 0, 0, l * 3]
    Y_l = landmarks[b, 0, 0, l * 3 + 1]
    Z_l = landmarks[b, 0, 0, l * 3 + 2]
    
  # Extract presence probability
  P_logit = presence[b, 0, 0, 0]
  P = 1/(1 + exp(-P_logit))
  
  # Extract tongueOut blendshape activation value
  B = blendshapes[b, 0]
```

# External References

* [MediaPipe FaceMesh V2 Model Card](https://storage.googleapis.com/mediapipe-assets/Model%20Card%20MediaPipe%20Face%20Mesh%20V2.pdf)
* [MediaPipe Face landmark detection guide](https://developers.google.com/edge/mediapipe/solutions/vision/face_landmarker#task_details)
* [MediaPipe FaceMesh V1 Paper](https://arxiv.org/pdf/1907.06724)
* [MediaPipe FaceMesh V1 Docs](https://github.com/google-ai-edge/mediapipe/blob/76f1d78eb1c350f882536adfa84d248ad0c84a60/docs/solutions/face_mesh.md)
* [MediaPipe Repository](https://github.com/google-ai-edge/mediapipe)
* [Facial Landmarks Mapping Image](https://storage.googleapis.com/mediapipe-assets/documentation/mediapipe_face_landmark_fullsize.png)
* [Facial Landmarks Connections (Python)](https://github.com/google-ai-edge/mediapipe/blob/76f1d78eb1c350f882536adfa84d248ad0c84a60/mediapipe/tasks/python/vision/face_landmarker.py#L109-L2869)

# Models

* [Face Landmarker Model Bundle](https://developers.google.com/edge/mediapipe/solutions/vision/face_landmarker#models)
> To obtain the FaceMesh V2 tflite model, `unzip` the model bundle (the `.task` file). Upon extraction, you can find the FaceMesh V2 model under the name `face_landmarks_detector.tflite`.

# Tensor Decoders

* https://github.com/yakhyo/uniface/blob/main/uniface/landmark/facemesh.py
* https://github.com/cornpip/mediapipe_face_mesh/blob/master/src/mediapipe_face_mesh.cc
* https://github.com/ailia-ai/ailia-models/blob/master/face_recognition/facemesh_v2/facemesh_v2.py
* https://github.com/terryky/tflite_gles_app/blob/master/gl2facemesh/tflite_facemesh.cpp
* https://github.com/PINTO0309/facemesh_onnx_tensorrt/blob/main/facemesh_postprocess.py


[landmarks]: /tensors/facemesh-v2-out-landmarks.md
[presence]: /tensors/facemesh-v2-out-presence.md
[blendshapes]: /tensors/facemesh-v2-out-blendshapes.md
