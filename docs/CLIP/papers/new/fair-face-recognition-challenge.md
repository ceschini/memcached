# FairFace Challenge at ECCV 2020: Analyzing Bias in Face Recognition

The paper describes a new challenge for face recognition based on bias metrics and a new biased and non-balanced dataset. First, they define the tasks of face recognition as two composed tasks, face identification and verification. Face identification is a 1-to-n process where a given face is matched against a given identity form n other faces form a database or something similar. Face verification is a 1-to-1 process where a single face is matched against another given face, in a much simpler matter.

The FairFace Challenge is a face recognition challenge where participants must achieve high accuracy while minimizing bias for gender and color of skin. They give a template for bias evaluation, and reported methods with 99% accuracy and yet poor bias mitigation scores.

This paper could provide a better baseline for gender and race classification, and we could explore its metrics to employ rather than simple accuracy.