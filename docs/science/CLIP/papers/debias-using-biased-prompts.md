# Debiasing Vision-Language Models via Biased Prompts

The authors developed a projection matrix capable of debiasing text prompts of VLMs such as CLIP. By doing so, "a photo of a male doctor" and "a photo of a female doctor" can keep similar embeddings. It also works on generative models like Stable Diffusion, because it works with the same text prompt embeddings as CLIP.

> \[…] we define a set of biased directions in the embedding using prompts that describe the biases.
> 
>  However, solely relying on prompts to define biased directions may be unstable and noisy [15]. To address this issue, we propose a calibration loss that minimizes the discrepancy of a pair of prompt embedding.

They propose a calibration loss that minimizes the discrepancy between a pair of prompt embeddings that contain biases. By doing so, the embeddings of both male and female versions of a given prompt should be similar, and as such, debiased. Only focusing on the text embedding was sufficient to improve group robustness of zero-shot models, verifying the other works declaration of parallelism between VLMs and LLMs biases.

The datasets used here were waterbird, CelebA and FairFace. The metric is the cosine similarity between the model weights and a corresponding spurious feature.