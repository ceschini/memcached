# Language Models are Few-Shot Learners (GPT-3)

This is the GPT-3 paper. Here they explore concepts like meta-learning and unsupervised learning with transformer language models.

They start by listing some problems with the pre-training fine-tuning paradigm, in which even though the architecture is task-agnostic, there is still a need for task-specific datasets and task-specific fine-tuning in order to achieve strong performance on a desired task.

Some of the key problems with this approach are that the need for a large dataset of labeled examples for every new task limits the applicability of language models; Also, the potential to exploit spurious correlations in training data fundamentally grows with the expressiveness of the model and the narrowness of the training distribution.

One possible route towards addressing these issues is **_meta-learning_** - which in the context of language models means the model develops a broad set of skills and pattern recognition abilities at training time, and then uses those abilities at inference time to rapidly adapt to or recognize the desired task. This is possible via something called **in-context learning**, by using the text input of a pre-trained language model as a form of task specification: the model is conditioned on a natural language instruction and/or a few demonstrations of the task and is then expected to complete further instances of the task simply by predicting what comes next.

It is as if the model recalls seeing something similar, and then by studying some examples or the text input, it can recover the correct samples for that specific task.

![[docs/img/meta-learning.png]]

GPT-3 is basically a transformer language model trained with 175 billion parameters and then measured by its in-context learning abilities.

