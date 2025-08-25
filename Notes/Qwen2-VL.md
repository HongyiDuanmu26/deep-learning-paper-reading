# [Qwen2-VL: Enhancing Vision-Language Model's Perception of the World at Any Resolution](https://arxiv.org/pdf/2409.12191)

The following work is based on Qwen-VL. Compared to version 1, version 2 has some improvements on: 1. The input image can be in dynamic resolution, while version 1 can take only fixed image resolution (224*224)
2. Version 2 can take videos (20min+) as inputs, 3. Version 2 can support European languages, Japanese, Korean, etc., while version 1 supports only English and Chinese.

Technically, there are several main differences compared to version 1:
1. The ViT encoder is much smaller, 675M, from DFN, while in version 1, it is 1.9B. Not sure why they used a smaller vision encoder. Version 1's LLM is Qwen-7B model and in version 2, they provide three options
Qwen-2B, Qwen-7B, Qwen-72B.
2. The adapter used in version 1 is removed in version 2. The absolute positional embedding is replaced with 2D-RoPE. After ViT, there is one layer MLP to reduce adjacent 2*2 tokens into one.
and ViT patch size is 14. So a 224*224 will be compressed to 66 tokens after vision encoder.
3. Multimodal-RoPE. The original 1D-RoPE was extended to 3D (temporal, height, width) to make it suitable for videos. Please see the number assignment rule in the image below. 
   <img width="2310" height="668" alt="image" src="https://github.com/user-attachments/assets/bb905135-091f-430c-ba22-3a4a74da1594" />
4. Video inputs are sampled two frames per second. To make video processing and image processing unified, the image inputs will be treated as two identical frames. One 3D conv with depth of two is used for vision inputs.
   The maximum length for video inputs is 16384 tokens.
5. Similar 3-step training configurations. Train only ViT in the first pre-training; Train both ViT and LLM in the second pre-training; Train LLM only in the third finetuning step.
   In the first step, 600B tokens are used in training, including image-text pairs, OCR, and image classification tasks. In version 1, 1.4B image-text pairs are used in this step.
   In the second step, 800B tokens are used with multi-tasks. In version 1, 75M multi-task data are used in this step.
   In the third step, dataset has pure text-based dialogue data, image question-answering, document parsing, multi-image comparison, video comprehension, video stream dialogue, and agent-based interactions. In version 1, 350k instruction tuning data are used in this step.
   The dataset size is much larger than used in version 1.

# Note
It is surprising to see that version 2 is trained on a much larger dataset compared to version 1. The architectural modification is trivial. LLM, VLM is much about engineering challenges on training efficiency, stability, etc. 
