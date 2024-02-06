# What is H.264?

## 4.1 Introduction

Here we will address what is H.264, what is the basic operations carried out and look in more depth inside the standard, examining what is contained within, what are the main sets of tools and components, etc.

## 4.2 What is H.264?

H.264 Advanced Video Coding can be defined by different views. It is an industry standard for video coding, but it is also a _format_ for coded video, and a set of tools for video compression.

### 4.2.1 A video compression format

H.264 is a method and a format for video compression, it specifies how the digital data should be compressed and how it should be defined in order for it to work. The gains of standardizing this type of operation is that different manufacturers can interoperate without having to develop the other end of products. For example, a DVD player manufacturer doesn't have to bother with the technology behind the DVD disks, because it follows a standardized format.

### 4.2.2 An industry standard

H.264 standard is contained in a document elaborated by two organizations, ITU-T and ISO/IEC. The document specifies what is an H.264 compressed syntax, and what are the required steps to decompressed it. It actually does not limit how this syntax will be made, leaving for the manufacturers of the encoder to develop and improve the best solution. 

![[h264-standard-covered-steps.png]]

### 4.2.3 A toolkit for video compression

H.264/AVC defines a set of tools for video compression that specifies how video should be represented and decoded. A sub-set of these tools must be available for the decoder in a so-called _profile_.

## 4.2 How does an H.264 codec work?

The encoder carries out prediction, transforming and encoding processes to produce a compressed H.264 bitstream. The decoder carries out complementary processes of decoding, inverse transform and reconstruction to produce a decoded video sequence. The resulting video is not identical to the source due to H.264 being a lossy compression format.

Based on the original video frame, the encoder will process the compressed bitstream and send it to the other end. Beyond that, it will also reconstruct the frame and save it in a coded picture buffer, CPB, in order to compress the following frames based on the reconstruction. A similar process is done at the decoder with a decoded picture buffer, DPB.

Data is processed in units of a **Macroblock**, 16 x 16 squared region of displayed pixels. In the encoder, a prediction macroblock is generated and subtracted, producing a residual that is transformed, quantized and bitstream encoded, ready to be sent to the decoder. Another process is followed along, where the quantized data are re-scaled and inverse transformed and added to the prediction macroblock to reconstruct a coded version of the original frame, which is stored for later predictions. 

![[typical-h264-encoder.png]]

Over at the decoder, the received bitstream is decoded, re-scaled and inverse transformed in order to form a decoded residual macroblock. The decoder generates the same prediction and adds the residual to form the final reconstructed macroblock.

![[typical-h264-decoder.png]]

### 4.3.1 Encoder processes

#### 4.3.1.1 Prediction

The encoder forms a prediction of the current macroblock based on previously-coded data, either from the current frame using **intra** prediction or from other frames that have already been coded and transmitted using **inter** prediction. The encoder subtracts the prediction from the current macroblock to form a **residual**$^1$.

The prediction methods supported by H.264 are more flexible than those in previous standards, enabling accurate predictions and hence efficient video compression. Intra prediction uses 16 x 16 and 4 x 4 block sizes to predict the macroblock from surrounding, previously coded pixels within the same frame. Inter prediction uses a range of block sizes from 16 x 16 down to 4 x 4 to predict pixels in the current frame from similar regions in previously coded frames.

---
$^1$ Finding a suitable inter prediction is often described as **motion estimation**. Subtracting an inter prediction from the current macroblock is **motion compensation**.

![[pred-flow-diag.png]]

![[intra-preds2.png]]

![[inter-preds2.png]]

#### 4.3.1.2 Transform and quantization

A block of residual samples is transformed using a 4 x 4 or 8 x 8 **integer transform**, an approximate form of the Discrete Cosine Transform (**DCT**). The transform outputs a set of **coefficients**, each of which is a weighting value for a standard basis pattern. When combined, the weighted basis patterns re-create the block of residual samples.

The output of the transform, a block of transform coefficients, is **quantized**, i.e. each coefficient is divided by an integer value. Quantization reduces the precision of the transform coefficients according to a *quantization parameter (QP)*. Typically, the result is a block in which most or all of the coefficients are zero, with a few non-zero coefficients. Setting QP to a high value means that more coefficients are set to zero, resulting in high compression at the expense of poor decoded image quality. Setting QP to a low value means that more non-zero coefficients remain after quantization, resulting in better image quality at the decoder but also in lower compression.

![[forward-transform.png]]

![[quant-example.png]]

#### 4.3.1.3 Bitstream encoding

The video coding process produces a number of values that must be **encoded** to form the compressed bitstream. These values include:

- Quantized transform coefficients
- Information to enable the decoder to re-create the prediction
- Information about the structure of the compressed data and the compression tools used during encoding
- Information about the complete video sequence.

These values and parameters, **syntax elements**, are converted into binary codes using **variable length coding** and/or **arithmetic coding**.

### 4.3.2 Decoder processes

#### 4.3.2.1 Bitstream decoding

A video decoder receives the compressed H.264 bitstream, decodes each of the syntax elements and extracts the information described above, such as quantized transform coefficients, prediction information, etc. This information is then used to reverse the coding process and recreate a sequence of video images.

#### 4.3.2.2 Rescaling and inverse transform

The quantized transform coefficients are **re-scaled**. Each coefficient is multiplied by an integer value to restore its original scale$^2$. The re-scaled coefficients are similar but not identical to the originals.

---
$^2$ This is often described as **inverse quantization**, but it is important to note that quantization is not a fully reversible process. Information removed during quantization cannot be restored during re-scaling.

![[inverse-transform.png]]

An inverse transform combines the standard basis patterns, weighted by the re-scaled coefficients, to re-create each block of residual data. These blocks are combined to form a residual macroblock.

#### 4.3.2.3 Reconstruction

For each macroblock, the decoder forms an identical prediction to the one created by the encoder using inter prediction from previously-decoded frames or intra prediction from previously-decoded samples in the current frame. The decoder adds the prediction to the decoded residual to **reconstruct** a decoded macroblock which can then be displayed as part of a video frame.

## 4.4 The H.264/AVC Standard

The current version of Recommendation H.264 is a document of over 550 pages. It contains normative content, essential instructions that must be observed by H.264 codecs, and informative content, guidance that is not mandatory. Chapter 7 talks about the semantics, the syntax definition and elements that the bitstream should be compliant. The decoder parsing process to extract syntax elements from the bitstream is defined in Chapter 9, and the decoding processes to create a decoded video sequence are described in Chapter 8.

## 4.5 H.264 Profiles and Levels

H.264 supports a portfolio of 'tools'. These are algorithms or processes for coding and decoding video and related operations. These include tools essential to any implementation of H.264 such as the basic 4 x 4 transform, optional or interchangeable tools such as CABAC or CAVLC entropy coding, tools for specific applications or scenarios such as the SI/SP tools which can support streaming video and tools not directly related to video coding such as VUI and SEI messages.

An encoder has considerable flexibility in choosing and using tools defined in the standard. A decoder may be capable of operating with a very limited set of tools, for example, if it is only required to decode bitstreams that use a subset of tools. H.264 **profiles** each define a specific subset of tools. An H.264 bitstream that conforms to a particular profile can only contain video coded using some or all of the tools within the profile; a profile-compliant decoder must be capable of decoding with every tool in the profile. The profiles therefore act as constraints on the capabilities required by a decoder.

The Main Profile is perhaps the most widely used at the present time, with tools that offer a good compromise between compression performance and computational complexity. The Constrained Baseline Profile is a subset of the Main Profile that is popular for low-complexity, low-delay applications such as mobile video.

A second constraint is the amount of data that a decoder can handle, which is likely to be limited by the processing speed, memory capacity and display size at the decoder. An H.264/AVC **level** specifies an upper limit on the frame size, processing rate (number of frames or blocks that can be decoded per second) and working memory required to decode a video sequence. A particular decoder can only decode H.264 bitstreams up to a certain combination of Profile and Level.

![[profiles-and-levels.png]]

The Profile and Level of an H.264 bitstream are signalled in the Sequence Parameter Set (Chapters 5 and 8).

## 4.6 The H.264 Syntax

H.264 provides a clearly defined format or **syntax** for representing compressed video and related information.

![[h264-syntax.png]]

At the top level, an H.264 **sequence** consists of a series of 'packets' or Network Adaptation Layer Units, NAL Units or NALUs. These can include **parameter sets** containing key parameters that are used by the decoder to correctly decode the video data and **slices**, which are coded video frames or parts of video frames.

At the next level, a **slice** represents all or part of a coded video frame and consists of a number of coded **macroblocks**, each containing compressed data corresponding to a 16 x 16 block of displayed pixels in a video frame.

At the lowest level, a macroblock contains type information describing the particular choice of methods used to code the macroblock, prediction information such as coded motion vectors or intra prediction mode information and coded residual data.

***Example***

An H.264 decoder receives the following series of bits, describing an inter-coded macroblock or P MB coded using context-adaptive VLCs:

110111110001110001110010 (alternate syntax elements highlighted).

The header elements, the first eight bits and six syntax elements, indicate the macroblock type (P, one motion vector), the prediction parameters, in this case the x and y components of the motion vector, coded differentially from previously-received vectors, the coded block pattern which indicates that only one of the 4 x 4 blocks in the MB actually contains residual data and the quantization parameter, coded differentially from the previous macroblock.

![[header-elements.png]]

The next 16 bits represent 5 syntax elements that specify the contents of the first and only non-zero coefficient block.

![[coef-block-info.png]]

Decoding the macroblock proceeds as follows:

- Extract the residual luma and chroma coefficient blocks
- Calculate the actual quantization parameter QP
- Carry out the inverse quantization and inverse transform processes to produce the residual sample blocks
- Calculate the actual motion vector
- Produce a prediction macroblock
- Add the prediction macroblock to the residual samples to reconstruct the decoded macroblock
- Store as part of the decoded frame for display and/or for further prediction.

![[p-macroblock-decoding.png]]

## 4.7 H.264 in practice

### 4.7.1 Performance

Perhaps the biggest advantage of H.264 over previous standards is its compression performance. Compared with standards such as MPEG-2 and MPEG-4 Visual, H.264 can deliver:

- Better image quality at the same compressed bitrate, or
- A lower compressed bitrate for the same image quality.

For example, a single-layer DVD can store a movie of around two hours' length in MPEG-2 format. Using H.264, it should be possible to store four hours or more of movie-quality video on the same disk, i.e. a lower bitrate for the same quality. Alternatively, H.264 compression format can deliver better quality at the same bitrate compared with MPEG-2 and MPEG-4 Visual.

![[visual-comparison-formats.png]]

The improved compression performance of H.264 comes at the price of greater computational cost. H.264 is more sophisticated than earlier compression methods, and this means that it can take significantly more processing power to compress and decompress H.264 video.

### 4.7.2 Applications

As well as its improved compression performance, H.264 offers greater flexibility in terms of compression options and transmission support. An H.264 encoder can select from a wide variety of compression tools, making it suitable for applications ranging from low-bitrate, low-delay mobile transmission through high definition consumer TV to professional television production. The standard provides integrated support for transmission or storage, including a packetized compressed format and features that help to minimize the effect of transmission errors.

H.264/AVC has been adopted for an increasing range of applications, including:

- High Definition DVDs (Blu-Ray)
- High Definition TV broadcasting in Europe
- Apple products including iTunes video downloads, iPod video and MacOS
- NATO and US DoD video applications
- Mobile TV broadcasting
- Many mobile video services
- Many internet video services
- Videoconferencing
- Consumer camcorders.

