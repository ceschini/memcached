# Learning Transferable Visual Models From Natural Language Supervision

## First pass

### Context

They explore how computer vision models can leverage the capabilities of NLP to train task-agnostic and zero-shot learning using text-to-image and web crawling. The related papers include those that explore how captions and labels can improve computer vision models in classification tasks. That is, papers that use *natural language supervision for image representation learning*.

### Contributions

They trained a simplified version of the *ConVIRT* from scratch using 400 million image-text pairs and showed how it can zero-shot classify more specific tasks.

Basically they grabbed an existing technique and explored its limited performance due to lack of computation, leveraging openAI processing capabilities.