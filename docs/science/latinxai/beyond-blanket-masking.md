# Beyond Blanket Masking: Examining Granularity for Privacy Protection in Images Captured by Blind and Low Vision Users

# Reading Notes

Visual assistant systems leverage VLMs but can also expose personal information, specially when used by vision impaired users that don't even know that they are seeing personal data. The authors propose a framework to improve personal information masking by a fine-grained segmentation rather than coarse-grained, preventing non-personal information from being masked by accident.

They say that their system preserves +26% of image content, provide useful responses 11% and identify image content by 45%, but doesn't state this numbers compared to what.

They argue that by specifically masking only the critically private data, they can enhance the ability of VLMs to help the blind and visually impaired users. For example, instead of masking the whole financial form, they mask only the high-risk information, they can still provide form information for the Q/A system.

They evaluate how well their system can mask fine-grained information, and how this fine-grained masked images can enhance Q/A performance of VLMs.

In the end of the introduction they properly state their gains: compared to the ratings of full-object masking.

they calculated a risk score based on the Identity Ecosystem Graph, and the Identity Threat Assessment and Prediction (ITAP) dataset, which contains 5,906 news reports of identity theft. By linking each kind of information with its resulting consequences they established a risk score to determine the relevancy of each personal information.

Based on the Identity Threat Assessment and Prediction (ITAP) dataset, which contains 5,906 news reports of identity theft, a risk score is computed and used to evaluate if each piece of detected personal information is of high or low risk, masking those of high risk while leaving the low risk untouched.

They leverage VLMs to detect possible private information, and to draw a bounding box around it. Then, they crop it and rotate it to help the text detection. They use OCR to recognize texts and another VLM to enhance the bounding box and correctly align to the original coordinate space. Finally, they assess the risk score of the detected information, if it is considered of high-risk, the region is masked.

To experiment they framework they employ the BIV-Priv-Seg dataset, containing images captured by blind and low-vision users on private objects with 'props' information. They compare the proposed framework with two other strategies: complete masking with full-object masking and fine-grain masking of **all** information related to private object.

The images processed by these three distinct approaches are then used in a VQA task, synthesized by the authors. The first step is Object Recogition, where they ask a VLM if a personal object is present in the image, computing model's likelihood of answering "yes". The final step is to ask BLV users relevant questions based on the VizWiz dataset. Since they don't have ground-truth asnwers for these questions, they only ask if the question is answerable or not.

They conducted a human annotation study to rate the quality of those three different masking strategies. The performance could not be improved when the private object was not the focus of the question.

For some cases, their framework performed better, but for some cases "many questions relate to information that falls near the boundary of being high-risk".

## Summary and contribution

Recently, visual assistant systems powered by visual language models (VLMs) have become more prevalent. However, specially for the visually impaired users, the presence of private information in the images being capture can negatively impact the capabilities of such systems. Common approaches for such privacy concerns involve coarse-grained masks that, while protecting private information, can potentially hide the whole private object and, by doing so, prevents VLMs for answering questions related to the target object. To address this issues, the paper proposes a fine-grained privacy protection framework capable of hiding private information whilst keeping the private object visible and detectable by the visual assistant system. The framework leverages VLMs and a computed risk score to detect, segment, orient and efficiently mask high-risk private information in a fine-grained level, keeping the low-risk information and the whole object still visible. Such approach was able to preserve more image content, enhance the ability of VLMs to provide useful responses and to identify the image content.

## Strengths

Consider the significance of key ideas, experimental or theoretical validation, writing quality, data contribution. Explain clearly why these aspects of the paper are valuable.

The paper addresses an important and noble research field. VLMs are becoming more prevalent in a variety of areas, and their ability to enhance visual assistant systems is really promising. The paper is well-written and objectively describes its contributions and proposed framework. The experimental setup is well-defined and successfully evaluates the capabilities of the proposed framework.

## Weaknesses

The paper didn't further investigate the shortcomings of its proposed method, only reporting where it didn't perform so well, and superficially stating that it was due to borderline confidence. The results reported in its abstract was a little confusing, and only by reading through the introduction it became clear that their results were based on the common approach of full-object masking. Finally, it also lacked a session or sentence about future works.

## Justifying the rating

What are the most important factors in your rating?

The paper proved to be relevant, promising, and well-written. The proposed framework is clever, simple and efficient, and leverages the same VLMs tools it is set to help improve.

## Comments to authors

The results in the abstract should state that they are compared to full-object masking. A comparison of the proposed framework with a standard deep learning network should prove interesting and could further highlight the contribution. Finally, a deep exploration of the shortcomings of the proposed method and some hints of possible future works would help improve the robustness of the paper.