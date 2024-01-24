# Optical Flow Estimation using a Spatial Pyramid Network

The authors proposed the adoption of a classical optical flow method with the addition of deep learning, in order to improve large motion estimation at much faster processing times.

Classical flow methods leverage highly engineered manual filters in order to estimation motion information, while recent work with machine learning methods tries to estimate motion purely based on neural networks. The author's method is a combination of both worlds, that can leverage dynamic learning filters of modern methods with the ability to deal with large motions of the classical spatial pyramid structure.

The presented method trains one deep network per level of the pyramid, with the goal of computing the flow update between layers. This proved more efficient and of better quality than the previous deep learning method of FlowNet.