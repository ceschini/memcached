# FairFace Challenge at ECCV 2020: Analyzing Bias in Face Recognition

The paper describes a new challenge for face recognition based on bias metrics and a new biased and non-balanced dataset. First, they define the tasks of face recognition as two composed tasks, face identification and verification. Face identification is a 1-to-n process where a given face is matched against a given identity form n other faces form a database or something similar. Face verification is a 1-to-1 process where a single face is matched against another given face, in a much simpler matter.

The FairFace Challenge is a face recognition challenge where participants must achieve high accuracy while minimizing bias for gender and color of skin. They provide a **fairness measure template**, and reported methods with 99% accuracy and yet poor bias mitigation scores.

This paper could provide a better baseline for gender and race classification, and we could explore its metrics to employ rather than simple accuracy.

> We propose a **new evaluation metric** derived from a causal model by means of a causal effect of protected attributes to the accuracy of the algorithm.

The authors report that most datasets with ethnicity groups as labels suffer from an arbitrary definition of those classes, and that they rely on the judgement of annotators. Such things won't happen with skin color, and that's why they chose it.

They argue that traditional measures of fairness are based on calculating certain statistics related to the error rate of the algorithm for a given group of protected attributes. These measures are easy to calculate but can't provide any background in statistics, resulting in critical contradictions between them. 

State-of-the-art measures are now being based on **causal inference**. Given a "model of the world" as a causal diagram, the actual measure is then derived from it, based on the causal effect of the protected attributes to the algorithm accuracy, or using a counterfactual sentence such as "would the decision remain had the value of the protected attribute be different but everything else remained the same?"

The highlight of these measures is that they encode ethical views in a simple way, and can be evaluated and changed if they prove inadequate.

The authors divide bias mitigation methods based on the area of model deployment. They target pre-processing, in-processing and post-processing. The most relevant to our use case is the post-processing one:

> Examples of post-processing techniques are re-normalizing the similarity score of two features vectors based on the demographic groups of the corresponding images. \[51]

For the final ranking evaluation, besides accuracy, the lowest positive pairs and highest negative pairs were evaluated with the bias metrics.

![[fairface-challenge-image-examples.png]]

The best methods leveraged ensemble models, external data, bias aware loss functions and most importantly, **post-processing bias mitigation techniques**.

![[fairface-challenge-general-info.png]]

## References

- \[51] Post-comparison mitigation of demographic bias in face recognition using fair score normalization.
