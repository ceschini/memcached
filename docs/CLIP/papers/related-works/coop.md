# Learning to Prompt for Vision-Language Models

Vision-Language Models perform well on downstream tasks by aligning visual and textual features. To do so, manual prompts must be formulated to better encapsule the class labels being targeted. But this requires domain expertise and time to do it correctly. The authors propose a method to *learn* the better prompt used in such tasks. They called it *Context Optimization* (CoOp), a simple approach specifically adapting CLIP-like vision-language models.

CoOp models a prompt's ***context words*** with learnable vectors while the entire pre-trained parameters are kept fixed. There are two implementations, an unified context and class-specific context. The former is better to classes that share a common context, the latter is better to classes that can differ in context, and as such will learn a specific set of context tokens for each class, more suitable for fine-grained categories.

> CoOp requires as few as one or two shots to beat hand-crafted prompts with a decent margin.

**Prompt engineering vs Context Optimization (CoOp)**: The former needs to use a held-out validation set for words tuning, which is inefficient, the latter automates the process and requires only a few labeled images for learning.

During training, CoOp will only learn the context vectors for the textual prompts, while keeping the entire pre-trained parameters fixed.

> The gradients can be back-propagated all the way through the text encoder, distilling the rich knowledge encoded in the parameters for learning task-relevant context.

(Good explanation of how CLIP works)

## Context Optimization

In CoOp, the prompt is designed as a sequence of learnable vectors, followed by a target class. This target class is a word token that will be replaced from its corresponding word embedding vector by the text encoder. The goal is to learn the best combination of such vectors in order to predict the correct class name.

This class token can be placed at the end of the sequence, or in the middle. The advantage of a middle class token is the ability to fill the latter word cells with supplementary descriptions, such as "a type of food", or cut off the sentence earlier by using termination signal such as full stop.

The other option is **Class-Specific Context** (CSC), where context vectors are independent to each class, and can be particularly useful for some fine-grained classification tasks.

**Training** is performed by minimizing the standard classification loss based on the cross-entropy, *and the gradients can be back-propagated all the way through the text encoder, making use of the rich knowledge encoded in the parameters to optimize the context*.

>Despite being a learning-based approach, CoOp performs much better in domain generalization than manual prompts.

>Though the performance is excellent, the results of CoOp are relatively difficult to interpret.

> The experiments also reveal that CoOp is sensitive to noisy labels given the weak performance on Food101.

