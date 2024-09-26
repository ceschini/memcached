# Transformers: Attention is All you Need

The state-of-the-art in machine translation was recurrent neural networks, but they are highly sequential and suffer from long sentences. The idea of transformers is to ditch recurrence and convolutions, instead relying entirely on attention mechanisms to map the relationships between input and output sequences. The authors showed how it is way faster and efficient by training for shorter periods of time and achieving state-of-the-art performance.

Self-attention, or intra-attention is an attention mechanism relating different positions of a single sequence in order to compute a representation of it.

The baseline models for Transformers were the competitive neural sequence transductions models that have an encoder-decoder structure. In this case, the encoder maps an input sequence into continuous representations that are fed to the decoder in an auto-regressive way, where the current symbol is decoded using the previously generated symbols as an additional input.

> The Transformer follows this overall architecture using stacked self-attention and point-wise, fully connected layers for both the encoder and decoder.

Both encoder and decoder stacks are composed of a fixed number of identical layers. Each layer have two sub-layers, a multi-head self-attention mechanism and a simple position-wise fully connected feed-forward network. The decoder have a third sub-layer with a multi-head attention mechanism responsible to process the output of the encoder stack.

![[transformers-architecture.png]]

An attention function can be described as mapping a query and a set of key-value pairs to an output, where the query, keys, values and output are all vectors. The output is computed based on a weighted sum of the values, where the weight assigned to each value is computed by a *compatibility function* of the query with the corresponding key.

So for each value, computes its weight based on its corresponding key and the target query, and then sum it all together.

Transformers attention function is called "Scaled Dot-Product Attention", and is a dot-product (multiplicative) attention computed on a set of queries and key-values packed together into matrices.

This attention function is the basis for the Multi-Head Attention, a parallel attention operation based on the linear projections of the queries, keys and values. Allowing the model to jointly attend to information from different representation subspaces at different positions.

![[scaled-attention-and-multi-head.png]]
