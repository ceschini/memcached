# Training data-efficient image transformers & distillation through attention

**Distillation**: Knowledge or Model distillation is the process of transferring knowledge from a large model to a smaller one without loss of validity. This makes them easy and less expensive to be evaluated. It is referred as a training paradigm in which a *student* model leverages "soft" labels coming from a strong *teacher* network. It can also be used to transfer inductive biases from a convolutional model as a teacher.

The authors propose adding a distillation token along with the initial embeddings of patches and class token. The new token is used simillarly to the class token: it interacts with other embeddings through self-attention, and is output by the network after the last layer. Its target objective is given by the distillation component of the loss. The distillation embedding allows the model to learn from the output of the teacher, as in a regular distillation, while remaining complementary to the class embedding.

(**This paper has a good overall of the vision transformers so far**)
