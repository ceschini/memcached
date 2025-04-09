# Visual Prompt Tuning (VPT)

The authors argue that even though foundation models are the standard pre-trained weights, they require a full fine-tuning in order to be efficient in downstream tasks. This is expensive and an often infeasible proposition. So they propose a new simple and efficient method to adapt transformer models for downstream vision tasks, called Visual-Prompt Tuning (VPT).

VPT injects a small number of learnable parameters into Transformer's input space and keeps the backbone frozen during the downstream training stage. This is applied directly to the Vision Transformer (ViT) architecture and as such would be a different approach to be leveraged by the CLIP model, but only if it were possible to adapt the already pre-trained openCLIP models.

