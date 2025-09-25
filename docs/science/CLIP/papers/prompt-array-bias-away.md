# A Prompt Array Keeps the Bias Away

The authors argue that beyond debiasing solutions, there is also a necessity for bias measurement. They explored different metrics for bias and proposed a framework of image-text contrastive loss with an array of learnable tokens prepended to text embeddings in order to reduce bias without image degradation.

They established their own framework for measuring the degree of bias. Furthermore, they also investigated the suitability of well established metrics from NLP, such as WEAT and ranking metrics.

## Bias Metric Framework

Consider a dataset of image-attribute pairs (I, A) where I is an image and A is its corresponding attribute. For example, an image of a face with a gender attribute label.

Suppose there is a set of sensitive text queries T, with corresponding concepts C, such as "a photo of a good person", "a photo of a bad person". Here the concepts are C = {good, bad}

The goal is to learn a joint vision-language model that outputs similar distribution of scores across attributes for a given text query, which _should_ be unrelated to demographic affiliation (gender and race).

