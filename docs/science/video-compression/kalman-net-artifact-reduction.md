# Deep Non-Local Kalman Network for Video Compression Artifact Reduction

In this paper, the authors developed a framework that combines both model and learning methods in order to improve the quality of compressed frames. Their key assumption is that instead of using the noisy previous *decoded* frames as temporal information, the less noisy previous *restored* frame is employed in a recursive way, which provides the potential to generate high quality restored frames.

![[docs/img/kalman-pipeline.png]]

They also exploited the non-local prior information for aligning temporal frames, instead of the commonly used optical flow. This is due to the fact that the quality of the restored frame rely heavily on the accuracy of the optical flow estimation, that can't solve complex cases. Instead, non-local priors can capture the similarity between two neighboring frames, in a more robust and lightweight way.

![[docs/img/kalman-frame-align.png]]

The proposed framework was designed as a post-processing module that can enhance the quality of compressed frames and easily extend to different algorithms.