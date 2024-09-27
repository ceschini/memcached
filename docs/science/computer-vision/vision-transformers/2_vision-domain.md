# End-to-end object detection with transformers

Object detection is the task to compute bounding boxes on objects in a scene. The standard approach involves a lot of post-processing and hand-design components. The authors propose to approach object detection as a direct [[set-prediction | set prediction]] problem, a task where a model predicts multiple elements whose ordering is not relevant for correctness. 

The authors propose DEtection TRansformer (DETR), an object detection model that leverages CNNs and transformers to directly predicts in parallel the final set of detections using a bipartite matching, that uniquely assigns predictions with ground truth boxes.

![[Pasted image 20240927105416.png]]

Their approach simplifies the detection pipeline by dropping multiple hand-designed components that encode prior knowledge, like spatial anchors or non-maximal suppresion.

