# Text as Images in Prompt Tuning for Multi-Label Image Recognition

## First-pass

### Category

This is a Improvement Paper, describing how it is effectively better than the current State-of-The-Art.

### Context

It is related to prompt tuning of Visual Language (VL) models. They compare it to CoOp, CoCoOp and DualCoOp papers. Their theory is that it is feasible to use text instead of images for prompt tuning, because by using contrastive loss they can align image and text embeddings into a shared space.

### Correctness

To validate their assumptions we will have to read the referenced papers about contrastive loss and others. But with our limited knowledge it appears to be valid.

### Contributions

The paper's main contributions are the text-to-image prompt tuning, capable of beating zero-shot CLIP models in multi-label classification. They also provide the capabilities of an off-the-shelf integration with other kinds of prompt tuning, like the ones described in the CoOp papers above.

### Clarity

For me it is not well written, I know that I lack some background to understand the more technical terms, but some sentences just doesn't look right. Even so, I acknowledge it as a good overall paper.

## Second pass

![contrastive loss shared space](/imgs/shared_space.jpg)
Using contrastive loss* to align image and text embeddings into a *shared space*.
