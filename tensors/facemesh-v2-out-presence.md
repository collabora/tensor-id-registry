# Classification

- tensor-group: no
- layer-type: output
- use-case: face-landmarks
- part-of-tensor-groups:
  - [facemesh-v2-out](/tensor-groups/facemesh-v2-out.md)

# Description
The face presence tensor output of the MediaPipe FaceMesh V2 model contains the face flag, that is, the logit of the probability of a face being present in the input image.

## Face Presence Tensor

- tensor-shape: BATCH_SIZE x 1 x 1 x 1
- tensor-datatype: float32
- tensor-id: facemesh-v2-out-presence
- memory-layout: row-major order

Where:
* BATCH_SIZE is the size of the batch

### Known Aliases
* Identity_1

# Encoding

Scheme: [P]

- P = face flag

Memory layout of tensor data:

| Address | Symbol | Value     | Comment                            |
|---------|--------|-----------|------------------------------------|
| 0x0     | P      | face flag | tensor-start, complete tensor data |
| 0x4     | -      | -         | tensor-end                         |

# External References

* [MediaPipe FaceMesh V2 Model Card](https://storage.googleapis.com/mediapipe-assets/Model%20Card%20MediaPipe%20Face%20Mesh%20V2.pdf)
* [MediaPipe Face landmark detection guide](https://developers.google.com/edge/mediapipe/solutions/vision/face_landmarker#task_details)
* [MediaPipe Repository](https://github.com/google-ai-edge/mediapipe)

# Models

* [Face Landmarker Model Bundle](https://developers.google.com/edge/mediapipe/solutions/vision/face_landmarker#models)
> To obtain the FaceMesh V2 tflite model, `unzip` the model bundle (the `.task` file). Upon extraction, you can find the FaceMesh V2 model under the name `face_landmarks_detector.tflite`.

# Tensor Decoders

* https://github.com/google-ai-edge/mediapipe/blob/d652c28b7e27d04d49dda53dd359a98b881848ec/mediapipe/modules/face_landmark/face_landmark_cpu.pbtxt#L126-L135
* https://github.com/yakhyo/uniface/blob/main/uniface/landmark/facemesh.py
* https://github.com/ailia-ai/ailia-models/blob/master/face_recognition/facemesh_v2/facemesh_v2.py
