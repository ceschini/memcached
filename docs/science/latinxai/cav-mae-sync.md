# CAV-MAE Sync: Improving Contrastive Audio-Visual Mask Autoencoders via Fine-Grained Alignment

# First pass

In this paper, the authors proposed an improvement for the CAV-MAE framework where a more fine-grained synchronization is performed between audio e vision inputs. In the original CAV-MAE, a single video frame was related to 10 seconds of audio, and they force both contrastive and reconstruction objectives to share a single representation, creating competing optimization goals.

The authors also introduced some new features into the original CAV-MAE architecture. They added separate global tokens for each task and incorporated learnable register tokens that reduce semantic load on patch tokens while enabling finer-grained alingment.

They evaluated on some downstream tasks using popular datasets, with multiple state-of-the-art methods as baseline.