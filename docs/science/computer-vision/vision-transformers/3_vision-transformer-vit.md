# An image is worth 16x16 words: Transformers for image recognition at scale

Recent work has demonstrated interest in using attention-based mechanisms, but still CNN-like architectures and classic ResNet-like architecture represent the state of the art for large-scale image recognition.

The authors experiment with applying a standard Transformer directly to images, with the fewest possible modifications. To do so, they **split an image into patches and provide the sequence of linear embeddings of these patches as an input to a Transformer**. *Image patches are treated the same way as tokens (words) in an NLP application*.

When trained on mid-sized datasets, their performance is sub-optimal, due to a lack of inductive biases inherent to CNNs, such as translation equivariance and locality. However, when trained on larger datasets (14M-300M images), they were able to trump inductive bias and achieve performances comparable or better than the current state of the art.

## Vision Transformer (ViT)

The standard Transformer receives as input a 1D sequence of tokens embeddings. To handle 2D images, the image is reshaped into a sequence of flattened 2D patches, that are flattened and mapped to the transformer dimensions with a *trainable linear projection*. A class token is prepended to the sequence of embeddings, this is a learnable embedding whose state at the output of the Transformer encoder serves as the representation of image features. Along with each patch embedding is a standard learnable 1D position embedding to retain positional information.

![[docs/img/Pasted image 20240927160816.png]]
