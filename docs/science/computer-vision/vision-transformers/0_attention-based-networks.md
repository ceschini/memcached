# Describing Multimedia Content using Attention-based Encoder–Decoder Networks

## Structured output problems

Structured output problems where the task is to map the input to an output that possesses its own structure. The task is therefore not only to map the input to the correct output, but also to model the output structure.

A common example is machine translation, not only does the network translate from input language to output language, it must also understand the combination of these output words in order to make a coherent phrase, that is, understanding the structure of the output sentence.

One important aspect of virtually all structured output tasks is that *the structure of the output is intimately related to the structure of the input*. One central problem in such tasks is the *alignment*.

> At its most fundamental, the problem of alignment is the problem of how to relate subelements of the input to sub-elements of the output.

Alignment: **where should I look** in the input to generate the desired output? Not only to map the input to the correct output, but also to model the structure within the output sequence.

Taking image captioning generation as an example, the output sentence must have a description of the spatial relationships between elements of the scene. Not only there is a boy and a football, but the boy is kicking it on the air, doing some "embaixadinhas" hehe.

**For this, we need to *align* the output words to spatial regions of the source image**.

### Encoder-Decoder Framework

An encoder-decoder framework aims at handling the mapping between highly structured input and output. The encoder first reads the input data $x$ into a continuous-space representation $c$. Then the decoder generates the output $y$ conditioned on the continuous-space representation, or **context**, of the input.

![[encoder-decoder-framework.png]]

In the early adoptions of such framework, the context was a fixed-dimensional vector, regardless of the size of the input, and was not structured by design, but rather an arbitrary vector, which means that **there is no guarantee that this context vector preserved the spatial, temporal or spatio-temporal structures of the input**.

This characteristic rapidly degrades the performance of the encoder-decoder system as the length and/or complexity of the input data increases.

In tasks such as machine translation, the encoder needs to be able to summarize a variable-length source sentence into this fixed-dimensional context vector. In image captioning, even with a fixed-resolution image, the amount of information contained in each image may vary significantly. i.e. the number of objects on the scene.

Furthermore, by limiting the decoder the access to only the information compressed on an unstructured context vector, the generated output will have an extremely low interpretability.

## Attention mechanisms

To approach this alignment problems the authors propose an **attention mechanism**.

> Attention mechanisms are components of prediction systems that allow the system to sequentially focus on different subsets of the input

To decide which subset to look at, the system will choose based on a function of its current state and the previously attented subsets.

Attention mechanisms are good for mainly two reasons: They allow the system to process only a subset of the input, alleviating computational burdens, and by doing so focusing on distinct aspects of each subset, thus improving its ability to extract the most relevant information for each piece of the output.

> Soft attention mechanisms avoid a hard selection of which subsets of the input to attend and instead uses a *soft weighting* of the different subsets.

This approach does not offer computational gains, but allows efficient learning via gradient backpropagation.

## Attention-Based Multimedia Description

Multimedia description is the task of given an input multimedia data (image, video, audio or text), generate a description of such input in natural language. *This requires a model to capture the underlying, complex mapping between the spatio-temporal structures of the input, and the complicated linguistic structures of the output*. A neural network based approach to this problem could leverage an encoder-decoder framework.

## Attention mechanism for encoder-decoder models

With attention mechanisms the encoder can split the input information into a context vector set, in which each context vector inside this set can represent or summarize a certain spatial or temporal located information. For example, each context vector can summarize a phrase centered around a specific word in a source sentence, or a certain spatial location of an image.

The attention mechanism controls the input actually seen by the decoder, and it requires another neural network to work: the ***attention model***. This model is required to score each context vector with respect to the current hidden state of the decoder.

This type of scoring can be viewed as assigning a probability of being *attended* by the decoder to each context. Once the attention weights are computed, we use them to compute the next context *vector* $c^t$.

The output of the attention mechanism can be defined as a **soft or hard decision**. In the soft version, the attention model will summarize the hole context set into a weighted summed context vector, based on the attention weights. On the hard decision the higher scored context vector is the one to receive the attention or be attended by the decoder.

When the attended context vector is selected, the decoder will update its hidden state with that vector.

This mechanism allows the encoder to focus on a fixed amount of information around a particular region of the input, both spatially or temporally relevant.

## Image captioning with attention-based encoder-decoder models

Image captioning is a task in which a model looks at an input image and generates a corresponding natural language description. The encoder-decoder framework fits well with this task. The encoder will extract the continuous-space representation, or the context, of an input image, and pass it to the decoder that will generate a natural language description from it. Normally, the encoder is based on a CNN architecture, while the decoder commonly applies an RNN for it.

The usual encoder-decoder based image caption generation models use the activation of the last fully-connected hidden layer as the continuous-space representation, or the context vector, of the input image.

However, a recent work proposed to use the activation from the last *convolutional* layer of the pre-trained CNN as the context set. In this case, the context will consist of multiple vectors that correspond to different spatial regions of the input image, on which the attention mechanism can be applied. Also, by convolutioning and pooling, multiple context vectors will have overlapped regions that could help the attention mechanism to distinguish similar objects in an image, by comparing the context information with the whole image.

![[image-captioning-attention-codec.png]]

