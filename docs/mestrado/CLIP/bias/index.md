# Exploring CLIP Bias Reports

Here we will jot down occurrences and reports about CLIP bias on the already collected papers, followed by a brief survey on this issue.

## CLIP Paper

They started by probing the model for biases in the FairFace dataset. They explored how biased the model could be when adding beyond the standard classes, like male and female for gender, and white, black, Asian, Latino, etc. for race.

Furthermore, they observed that misclassifications were mainly attributed to underage people, they followed by adding "child" as a target label and saw how the misclassification dropped, insinuating how class design is crucial for models like CLIP not to be biased.

The authors followed by investigating possible bias in gender related classification. By using a dataset of people of the congress, they checked if well known biases would apply to CLIP as well. By tweaking the thresholds set for label probability, they encountered gender bias by applying "doctor" to men and "newsreader" to women on median thresholds, or "thief" to man and "nanny" to women on low thresholds.

Looked like they only worked on well established biases studies, leaving a lot of room for exploration.

## Oxford's Red Circles Paper

They just reported how adding red circles over man and women images excite the model to classifying them as missing person, suspect or criminal. This is due to the fact that CLIP probably learned with police footage that highlight person of interest this way. The authors followed the steps and parameters described on the CLIP paper above, like the FairFace dataset and the corresponding criminal and non-human labels.

## Parisot's Learning to Name Classes for VLMs

I will have to read the paper in order to understand how the authors suggest that we can use their work to interpret model biases.

## Snowballing

- [[prompt-array-bias-away| A Prompt Array Keeps the Bias Away: Debiasing Vision-Language Models with Adversarial Learning]]
- [[debiasing-vlms-via-bias | Debiasing Vision-Language Models via Biased Prompts]]
