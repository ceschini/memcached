# Learned Video Compression Via Joint Spatial-Temporal Correlation Exploration

The authors proposed an end-to-end framework that explore temporal and spatial correlation. They used an existing method for spatial correlation, and focused on temporal correlation. They represented temporal correlation from both first-order and second-order statistics. 

First-order temporal information is referred to as the *motion fields* (intensity, orientation) between consecutive frames, and can be described by optical flow. They trained a model using unsupervised learning to predict motion fields from sequential frames.

Second-order temporal information is *flow-to-flow* correlations, describing the object acceleration.