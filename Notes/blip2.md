# [BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models](https://arxiv.org/pdf/2301.12597)

As there are many pretrained good image encoders and LLMs, the authors want an easy way to utilize both at the same time, so that the system can process images and texts at the same time. Usually, end-to-end training requires too much computing cost and a very large dataset size.
So they proposed the Q-Former module and two-stage training methods to align vision features into the LLM text feature space. 
<img width="2176" height="661" alt="image" src="https://github.com/user-attachments/assets/633f1520-7fff-4700-bc75-134a5e4611ea" />

There is a 
