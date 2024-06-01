# FairerCLIP: Debiasing CLIP's Zero-Shot Predictions using Functions in RKHSs

Biases can be viewed based on dependencies between the data attributes. These attributes can be dependent or independent from each other. For example: Men have higher cheekbones than women, so High Cheekbones and Sex are dependent. On the other hand, both men and women can have any sort of hair color, and as such Hair Color and Sex are not dependent.

When the attributes are dependent they called this type of correlation as intrinsic dependence. When they are not dependent they called it a spurious correlation.

The authors claim that debiasing methods are commonly focused on the spurious correlations, requiring or not ground-truth labels and use iterative methods computationally costly. Their method is supposed to address all these issues.

They developed a method to learn the biases from text and image embeddings of CLIP and deploy their model as a post-processing stage after CLIP encoding and prior to cosine similarity classifiers.

For performance evaluation they use Average Accuracy, Worst-Group Accuracy and Gap, the difference between average and worst-group. Also, for spurious correlation they used MaxSkew and for intrinsic dependency they used Equal Opportunity Difference (EOD).