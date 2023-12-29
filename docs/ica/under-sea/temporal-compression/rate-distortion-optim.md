# Rate-distortion optimization for video compression

In this article, the authors give a deep review of methods, standards and explore the issue of choosing the best mode of operation for each part of a video frame.

"The most common 'baseline' JPEG scheme consists of breaking up the image in equal size blocks. These blocks are transformed and transmitted using variable length codes. We will refer to this kind of coding scheme as **INTRA**-frame coding, since the picture is coded without referring to other pictures in the video sequence."

However, we can improve the compression by leveraging the previous frames of a video sequence and its inherent temporal redundancy. Most of the pixels from one frame to the other are in a static position, and by assuming this, techniques of **INTER**-frame coding have been proposed.

A simple method of improving compression by coding only the changes in a video scene is called _conditional replenishment_ (CR), and it was the only temporal redundancy reduction method used in the first digital video coding standard, ITU-T Rec. H.120. This method consists of sending signals to indicate which areas of a picture can just be repeated, and sending new code information to replace the changed areas. CR allows the choice of two methods for representing these areas: a **SKIP** mode, in which the area can just be replaced by previously decoded frames, and an **INTRA** mode, in which the area must be encoded due to changes in pixel values. But CR coding has a significant shortcoming, its inability to provide an approximation. The region is skipped if all pixels are the same, or the region is replaced by INTRA mode due to pixel changes.

Adding a third type of **_prediction mode_**, in which a refining _frame difference_ approximation can be sent, results in a further improvement of compression performance. This concept can be further improved by adding _motion-compensated_ prediction (MCP). Most of the pixel changes in a frame is due to camera shifts or objects movements, and even the small movement can represent a significant shift of pixel values, especially on the edges of an object. So, by applying a small _shift_ or _displacing_ of a few pixels from the previous frame in to the next, we can further reduce the amount of information needed to perform frame difference approximation. 

This use of spatial displacement to form an approximation is known as **motion compensation**, and the encoder's search for the best spatial displacement approximation to use is known as **motion estimation**. The coding of the resulting difference signal for the refinement of the MCP signal is known as **displaced frame difference (DFD) coding**.

Hence, the most successful class of video compression designs are called hybrid codecs. And its name is due to its construction as a hybrid of motion handling and picture coding techniques. Its design and operation involves the _optimization_ of a number of decisions, including

1. How to segment each picture into areas,
2. Whether to replace each area of the picture with completely new INTRA-picture content,
3. If not replacing an area with new INTRA content 
	1. How to do motion estimation, i.e, how to select the spatial shifting displacement to use for INTER-picture predictive coding (with a zero-valued displacement being an important special case). 
	2. How to do DFD coding, i.e, how to select the approximation to use as a refinement of the INTER prediction (with a zero-valued approximation being an important special case), and
4. If replacing an area with new INTRA content, what approximation to send as the replacement content.

And so, the authors introduce the main problem of their work, which is **_What part of the image should be coded using what method?_** Hybrid codecs employ several modes of operation that are adaptively assigned to parts of the encoded picture, and there is a dependency between the effects of the two motion estimation and DFD coding stages of INTER coding. The modes of operation are in general associated with signal dependent _rate-distortion_ characteristics, and rate-distortion trade-offs are inherent in the design of each of these aspects.

**_The optimization of these decisions in the design and operation of a video coder is the primary topic of this article._**

According to [Wikipedia](https://en.wikipedia.org/wiki/Rate%E2%80%93distortion_optimization), Rate-distortion optimization (RDO) is a method of improving video quality in video compression. The name refers to the optimization of the amount of _distortion_ (loss of video quality) against the amount of data required to encode the video, the _rate_. While it is primarily used by video encoders, rate-distortion optimization can be used to improve quality in any encoding situation (image, video, audio or otherwise) where decisions have to be made that affect both file size and quality simultaneously.

