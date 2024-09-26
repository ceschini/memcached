# Describing Multimedia Content using Attention-based Encoder–Decoder Networks

Multimedia description: A general task in which a model generates a natural language description of a multimedia input such as speech, image and video.

Target: structured output problems, where the observed target is composed of multiple random variables that have a rich joint distribution, given the input. When the input and output are somehow related and both have a rich structure.

Machine translation: automatically translate a sentence from the *source language* to the *target language*.

Alignment: where should I look in the input to generate the desired output? Not only to map the input to the correct output, but also to model the structure within the output sequence.

Attention mechanisms: Components of prediction systems that allow the system to sequentially focus on different subsets of the input.

Gating units: in RNNs, this units avoid the vanishing gradient by adaptively controlling the flow of information across time steps.

## Attention mechanism for encoder-decoder models

With attention mechanisms the encoder can split the input information into a context vector set, in which each context vector inside this set can represent or summarize a certain spatial or temporal located information. For example, each context vector can summarize a phrase centered around a specific word in a source sentence, or a certain spatial location of an image.

The attention mechanism controls the input actually seen by the decoder, and it requires another neural network to work: the attention model. This model is required to score each context vector with respect to the current hidden state of the decoder. The higher scored context vector is the one to receive the attention or be attended by the decoder.

When the attended context vector is selected, the decoder will update its hidden state with that vector.

This mechanism allows the encoder to focus on a fixed amount of information around a particular region of the input, both spatially or temporally relevant.
