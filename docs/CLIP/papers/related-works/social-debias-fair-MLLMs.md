# Social Debiasing for Fair Multi-modal LLMs

The paper addresses the issue of social biases in MLLMs by introducing a new dataset called Counterfactual dataset with Multiple Social Concepts (CMSC), which is a more diverse and extensive dataset for model debiasing; and proposing an Anti-Stereotype Debiasing strategy (ASD). This method is equipped with the rescaling of the original autoregressive loss function as well as the improvement of data sampling to counteract biases.

**Metrics**: Maximum Skews accross different human attributes.
**Model Architectures**: LLaVA-7B, Qwen-VL-7B, Bunny
**Datasets**: FairFace

Here they explored social biased **concepts**, like occupation, instead of just gender or race. They  showed that exposing models to a richer set of social concepts **during training** is beneficial in MLLM debiasing.

They implemented the idea of *debiasing with the opposite of the social biases*. Their Anti-Stereotype Debiasing strategy (ASD) is equipped with two techniques, a new rescaled autoregressive loss function called Social Fairness Loss (SFLoss), and a data sampling method based on the bias level.

Basically they fine-tune a model by showing more images with underrepresented instances, and place larger emphasis on overlooked instances to weigh in heavily at loss optimization. This is done based on Skew indicators.

> In this manner, previously overlooked instances, e.g., *male nurses*, will receive more attention, serving as new 'alkaline' ones to counteract the bias of MLLMs that prefer *female nurses*.

Multi-modal Large Language Models (MLLMs) are different from Vision Language Models (VLMs) such as CLIP, because they are mostly trained in an autoregressive way rather than through contrastive learning.

## TODOs

- check differences between MLLMs and VLMs