# [BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models](https://arxiv.org/pdf/2301.12597)

As there are many pretrained good image encoders and LLMs, the authors want an easy way to utilize both at the same time, so that the system can process images and texts at the same time. Usually, end-to-end training requires too much computing cost and a very large dataset size.
So they proposed the Q-Former module and two-stage training methods to align vision features into the LLM text feature space. 
<img width="2176" height="661" alt="image" src="https://github.com/user-attachments/assets/633f1520-7fff-4700-bc75-134a5e4611ea" />

In side Q-Former, there are two branches, one for image features and one for text features. For image features, a set of queries (with a fixed number of 32) was designed to fetch information from image features by cross-attention inserted every other transformer block. The attention mask is used differently
when dealing with different tasks. In the first stage of training, a pre-trained frozen image encoder was used and Q-Former was pluged into it to learn to extract related vision information from image features. In this stage, the system was jointly optimized with three pre-training objectives.
1. Image-Text Contrastive Learning. We align the output query representation Z from the image transformer with the text representation t from the text transformer, where t is the output embedding of the [CLS] token. Since Z contains multiple output embeddings (one from each query), we first compute the
pairwise similarity between each query output and t, and then select the highest one as the image-text similarity. Unimodal self-attention mask was used, where the queries and text are not allowed to see each other.
2. Image-grounded Text Generation. Given input images as the condition, the system was asked to generate text. multimodal causal self-attention mask to control query-text interaction. queries can attend to each other but not the text tokens. Each
text token can attend to all queries and its previous text tokens.
3. Image-Text Matching.  It is a binary classification task where the model is asked to predict whether an image-text pair is positive (matched) or negative (unmatched). Bi-directional self-attention
mask was used where all queries and texts can attend to each other. The output query embeddings Z thus capture multimodal information. We feed each output query embedding into a two-class linear classifier to obtain a logit, and average the
logits across all queries as the output matching score.

When the first stage of training is done, second stage of training begins with a pre-trained frozen LLM. 
