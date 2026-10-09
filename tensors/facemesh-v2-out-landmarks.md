# Classification

- tensor-group: no
- layer-type: output
- use-case: face-landmarks
- part-of-tensor-groups:
  - [facemesh-v2-out](/tensor-groups/facemesh-v2-out.md)

# Description
The face landmarks tensor output of the MediaPipe FaceMesh V2 model has 478 3D landmarks flattened into a 1D tensor: (x1, y1, z1), (x2, y2, z2), ... The x and y coordinates correspond to the input image coordinates, the z coordinate is relative to the face center of mass and is scaled proportionally to the face width (under the [weak perspective projection camera model](https://en.wikipedia.org/wiki/3D_projection#Weak_perspective_projection)). FaceMesh V2 includes 10 more landmarks (for the irises) than FaceMesh V1's (without attention) 468 landmarks.

## Face Landmarks Tensor

- tensor-shape: BATCH_SIZE x 1 x 1 x 1434
- tensor-datatype: float32
- tensor-id: facemesh-v2-out-landmarks
- memory-layout: row-major order

Where:
* BATCH_SIZE is the size of the batch

### Known Aliases
* Identity

# Encoding

Scheme: [X, Y, Z]

- X, Y = landmark location in the pixel space of the input image
- Z = the depth of the landmark

The Z coordinate is scaled relative to X under the [weak perspective projection](https://en.wikipedia.org/wiki/3D_projection#Weak_perspective_projection). The z-origin is contained in the XY-plane intersecting the center of mass of the face mesh. Landmarks closer to the camera are smaller (more negative).

Each landmark point is a contiguous row of (X, Y, Z), where each component is a float32 value. A reference of the facial landmark mappings, connections and contours can be found in the External References section:

Memory layout of tensor data:

| Index       | Symbol | Value          | Comment                                  |
|-------------|--------|----------------|------------------------------------------|
| 0           | X      | landmark-0-x   | tensor-start, landmark-0-start           |
| 1           | Y      | landmark-0-y   | tensor-continue, landmark-0-continue     |
| 2           | Z      | landmark-0-z   | tensor-continue, landmark-0-end          |
| ...         | ...    | ...            | ...                                      |
| 478 * 3 - 3 | X      | landmark-477-x | tensor-continue, landmark-477-start      |
| 478 * 3 - 2 | Y      | landmark-477-y | tensor-continue, landmark-477-continue   |
| 478 * 3 - 1 | Z      | landmark-477-z | tensor-end, landmark-477-end             |

# External References

* [MediaPipe FaceMesh V2 Model Card](https://storage.googleapis.com/mediapipe-assets/Model%20Card%20MediaPipe%20Face%20Mesh%20V2.pdf)
* [MediaPipe Face landmark detection guide](https://developers.google.com/edge/mediapipe/solutions/vision/face_landmarker#task_details)
* [MediaPipe FaceMesh V1 Paper](https://arxiv.org/pdf/1907.06724)
* [MediaPipe FaceMesh V1 Docs](https://github.com/google-ai-edge/mediapipe/blob/76f1d78eb1c350f882536adfa84d248ad0c84a60/docs/solutions/face_mesh.md)
* [MediaPipe Repository](https://github.com/google-ai-edge/mediapipe)
- [Facial Landmarks Mapping Image](https://storage.googleapis.com/mediapipe-assets/documentation/mediapipe_face_landmark_fullsize.png)
- [Facial Landmarks Connections (Python)](https://github.com/google-ai-edge/mediapipe/blob/76f1d78eb1c350f882536adfa84d248ad0c84a60/mediapipe/tasks/python/vision/face_landmarker.py#L109-L2869)

# Models

* [Face Landmarker Model Bundle](https://developers.google.com/edge/mediapipe/solutions/vision/face_landmarker#models)
> To obtain the FaceMesh V2 tflite model, `unzip` the model bundle (the `.task` file). Upon extraction, you can find the FaceMesh V2 model under the name `face_landmarks_detector.tflite`.

# Tensor Decoders
https://github.com/yakhyo/uniface/blob/main/uniface/landmark/facemesh.py
https://github.com/cornpip/mediapipe_face_mesh/blob/master/src/mediapipe_face_mesh.cc
https://github.com/ailia-ai/ailia-models/blob/master/face_recognition/facemesh_v2/facemesh_v2.py
https://github.com/terryky/tflite_gles_app/blob/master/gl2facemesh/tflite_facemesh.cpp
https://github.com/PINTO0309/facemesh_onnx_tensorrt/blob/main/facemesh_postprocess.py
