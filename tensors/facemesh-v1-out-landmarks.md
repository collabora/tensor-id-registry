# Classification

- tensor-group: no
- layer-type: output
- use-case: face-landmarks
- part-of-tensor-groups:
  - [facemesh-v1-out](/tensor-groups/facemesh-v1-out.md)

# Description
The face landmarks tensor output of the MediaPipe FaceMesh V1 model has 468 3D landmarks flattened into a 1D tensor: (x1, y1, z1), (x2, y2, z2), ... The x and y coordinates correspond to the input image coordinates, the z coordinate is relative to the face center of mass and is scaled proportionally to the face width (under the [weak perspective projection camera model](https://en.wikipedia.org/wiki/3D_projection#Weak_perspective_projection)). FaceMesh V1, unlike FaceMesh V2, does not include the 10 iris landmarks.

## Face Landmarks Tensor

- tensor-shape: 1 x 1 x 1 x 1404
- tensor-datatype: float32
- tensor-id: facemesh-v1-out-landmarks
- memory-layout: row-major order

### Known Aliases
* conv2d_21

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
| 468 * 3 - 3 | X      | landmark-467-x | tensor-continue, landmark-467-start      |
| 468 * 3 - 2 | Y      | landmark-467-y | tensor-continue, landmark-467-continue   |
| 468 * 3 - 1 | Z      | landmark-467-z | tensor-end, landmark-467-end             |

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
