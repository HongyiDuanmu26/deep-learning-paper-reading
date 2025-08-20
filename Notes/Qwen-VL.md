# [Qwen-VL: A Versatile Vision-Language Model for Understanding, Localization, Text Reading, and Beyond](https://arxiv.org/pdf/2308.12966)

The Qwen-VL is the vision-language version of Qwen model. It is constructed based on Qwen model and modified to have the ability to process images.

It consists of: 1. Visual Encoder, a ViT module based on pretrained weights from Openclip’s ViT-bigG. About 1.9B parameters; 
2. Position-aware Vision-Language Adapter, a  single-layer cross-attention module initialized randomly, which reduces arbitrary image tokens to fixed 256 tokens by cross attention. 
2D absolute positional encodings are used to incorporate position information. 0.08B parameters;
3. LLM, Qwen-7B, 7.7B parameters;

<img width="1866" height="860" alt="image" src="https://github.com/user-attachments/assets/0afe9521-d695-43d5-a89f-8c7b6eea07b0" />

The training of Qwen-VL has 3 steps:
1. The first step is to train the ViT and the VL adapter and freeze the LLM. A large-scale, weakly labeled, web-crawled set of image-text pairs are used. Totally, 1.4 billion data cleaned from 5B data, with 77.3% English
(text) data and 22.7% Chinese (text) data.
2. The second step is to train the whole system with all three components' weights updated. High-quality and fine-grained VL annotation
data with a larger input resolution and interleaved image-text data are used. Tasks include Captioning, VQA, Grounding, Ref Grounding, Grounded Cap., OCR, Pure-text Autoregression.
Totally 72M data.
3. The third step is to optimize the language model and adapter module with the visual encoder frozen. Instruction fine-tuning to enhance
its instruction following and dialogue capabilities. The instruction tuning data amounts to 350k.
