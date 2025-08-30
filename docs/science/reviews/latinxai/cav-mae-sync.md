# CAV-MAE Sync: Improving Contrastive Audio-Visual Mask Autoencoders via Fine-Grained Alignment

# First pass

In this paper, the authors proposed an improvement for the CAV-MAE framework where a more fine-grained synchronization is performed between audio e vision inputs. In the original CAV-MAE, a single video frame was related to 10 seconds of audio, and they force both contrastive and reconstruction objectives to share a single representation, creating competing optimization goals.

The authors also introduced some new features into the original CAV-MAE architecture. They added separate global tokens for each task and incorporated learnable register tokens that reduce semantic load on patch tokens while enabling finer-grained alingment.

They introduced global tokens to improve the contrastive and reconstruction objectives, and added registers to the pipeline to further de-noise de ViT signal.

They evaluated on some downstream tasks using popular datasets, with multiple state-of-the-art methods as baseline.

CAV-MAE is the contrastive autoencoder framework that learns to reconstruct both visual and audio signals. It processes pairs of video-audio by sampling a video frame and using the full Mel spectogram of the corresponding audio. Both inputs are patchified, randomly masked and tokenized, passed through separate Vision Transformer (ViT) encoders, yielding modality-specific representations. A joint transformer layer processes these representations using three forward passes with shared weights but distinct layer normalizations, one for visual tokens, one for audio tokens, and one for their concatenation.

The model is trained with both reconstruction and contrastive objectives. The final learning objective balances contrastive alignment and masked reconstruction.

The authors argue that matching a full audio sequence to a single random frame creates a weak contrastive objective that results in imprecise audio-visual correspondences. To address this, they increase temporal granularity by extracting frame-aligned audio segments, leveraging natural temporal alignment between modalities.

They also argue that in the original architecture patches are optimized for both contrastive and autoencoder objectives using a shallow joint layer, that creates competing optimization goals. They propose strategies to disentangle these objectives and enhance model performance.

They experimented on three downstream tasks: cross-modal retrieval, classification and sound-prompted segmentation. They used the same pretrained model without task-specific modifications, and compared to SOTA methods on AudioSet, VGGSound and ADE20K_Sound.

## Summary and contribution

This paper proposes CAV-MAE Sync, a simple yet effective extension of CAV-MAE that leverages natural temporal alignment between modalities while relaxing joint representation constraints. Matching a full audio sequence to a single random frame creates a weak contrastive objective that produces imprecise audio-visual correspondences. To address this, an increased temporal granularity is proposed, by extracting frame-aligned audio segments and leveraging natural temporal alignment between modalities. Also, in the original architecture patches are optimized for both contrastive and autoencoder objectives using a shallow joint layer, that creates competing optimization goals. In this paper, strategies are proposed to disentangle these objectives and enhance model performance. Experiments were performed on three downstream tasks: cross-modal retrieval, classification, and sound-prompted segmentation. The same pre-trained model was used without task-specific modifications, and it was compared to state-of-the-art methods on the AudioSet, VGGSound, and ADE20K_Sound datasets.
## Strengths

The paper presents a novel method for multimodal audio-vision representation learning that improved the state of the art in three different downstream tasks, experimented with well-established datasets, and compared to a strong and diverse baseline. The methodology and theoretical foundation were well explained, with a descriptive overview of the proposed approach. The experiments correctly followed baseline protocols defined in the original work it proposed to improve.

## Weaknesses

Several follow-up works of CAV-MAE are mentioned, all supposedly facing the same key limitations of the original framework. However, there was no further investigation of how such methods work and how they perform in the downstream tasks evaluated. Beyond the SOTA models used as a baseline in the evaluation, these follow-up works could improve the robustness and strength of the improvement claims.

## Justifying the rating

The paper is in pristine shape and there were no apparent mistakes,  inconclusive claims, or assumptions. It is well-written and has a solid baseline, showing improvements over the state-of-the-art in multiple downstream tasks experimented over robust and popular datasets.
## Comments to authors

