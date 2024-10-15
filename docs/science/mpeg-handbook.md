# Chapter one: Introduction to compression

## What is MPEG?

MPEG is actually an acronym for the Moving Pictures Experts Group, formed by the ISO (International Standards Organization) to set standards for audio and video compression and transmission.

Compression is the event where a data source is transmitted from a *compressor*  to an *expander*, using a transmission channel. The goal here is to reduce the source data rate. The ratio between the source data rate and the channel data rate is called the *compression factor*, or *coding gain*. A compressor and expander in series is called a *compander*. They may as well be referred to *coder* and *decoder*, in which case the tandem pair may be called a *codec*.

![[docs/img/mpeg-1-compression.png | Compression pipeline]]

Where the encoder is more complex than the decoder, the system is said to be asymmetrical. MPEG works in this way. The encoder is algorithmic or adaptive, whereas the decoder is 'dumb' and carries out fixed actions. This is advantageous in applications such as broadcasting, where there are a few data sources with encoders, and a lot of receivers with decoders.

![[docs/img/mpeg-2-compression.png | Smart encoder, 'dumb' decoder]]

MPEG does not standardize the encoder, but instead define how decoders must interpret the bitstream, in order to be *compliant*. The decoder have to be able to interpret allowable bitstream, but encoders can still be compliant even if it produces a sub-set of possible bitstreams.

![[docs/img/mpeg-3-compression.png | MPEG defines the bitstream, not the encoder]]

MPEG goes beyond the bitstream definition, it also describes the protocol for multiplexing audio and video information, much like a television program.

## The MPEG-1, 2, 4 and H.264 contrasted

The first compression standard for audio and video was MPEG-1. It was basically designed to allow moving pictures and sound to be encoded into the bit rate of an audio Compact Disc. In order to meet the low bit requirement, MPEG-1 downsampled the images heavily as well as using picture rates of only 24-30 Hz, and the resulting quality was moderate. MPEG-2 was considerably broader in scope and of wider appeal. It was chosen as the compression scheme for both DVB (digital video broadcasting) and DVD (digital video disk). The goals of MPEG-3 were ready by the time MPEG-2 was standardized, so it was incorporated into it. MPEG-4 uses further coding tools with additional complexity to achieve higher compression factors than MPEG-2. It can also code computer graphics applications. The MPEG-4 decoder effectively becomes a rendering processor, and the compressed bitstream describes three-dimensional shapes and surface texture. In 2001 the Video Coding Experts Group (VCEG) from ITU (International Telecommunications Union) joined with ISO MPEG to develop a refined MPEG-4 version, known as H.264.

