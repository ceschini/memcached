# Oil Spill Detection

Os principais sistemas de detecção e identificação da poluição marítima por hidrocarbonetos são os sensores de **Radar de Abertura Sintética (SAR)**

## Refs

- **Oil Spill Identification from Satellite Images Using Deep Neural Networks.**
- Observing Marine Pollution with Synthetic Aperture Radar
- Compositional Oil Spill Detection Based on Object Detector and Adapted Segment Anything Model from SAR Images (2024)
- Semi-Supervised oil spill detection of SAR images based on pseudo-labelling (2024)
- Multiphysical Interpretable Deep Learning Network for Oil Spill Identification Based on SAR Images (2024)
- Diffusion-based Data Augmentation and Knowledge Distillation with Generated Soft Labels Solving Data Scarcity Problems of SAR Oil Spill Segmentation

Dataset: Sensor: COSMO-SkyMed

## Eyesea Method

![](eyesea-model-method.png)

## Conclusions

- Current segmentation models are not trainable by fine tuning for this task 
- combination of focal and dice loss have the best results for segmentation of oil spill
- Bigger datasets are key to improve this models
