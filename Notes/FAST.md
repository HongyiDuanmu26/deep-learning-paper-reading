# [FAST: Efficient Action Tokenization for Vision-Language-Action Models](https://arxiv.org/abs/2501.09747)

Authors argue that in autoregressive-like VLA systems, as the sampling frequency increases, the difference between actions decreases. So the system might learn to copy the previous action as the loss is marginal.
The tokenizer normally used is per-dimension, per-timestemp, binning, which will lead to too many tokens and a marginal difference between timestemp. So they want a high-information compression tokenizer and make neighboring tokens more different.

FAST tokenizer is fast and invertible. 
1. We first normalize the input actions, such that the 1st and 99th quantile of values in the training dataset for each action dimension maps to the range [−1,..., 1]
2.  discrete cosine transform (DCT)to get frequency domain.
3.  scale and round to quantize.
4.  Flatten along the frequency axis first and BPE to compress.
<img width="1060" height="622" alt="image" src="https://github.com/user-attachments/assets/5d4ae10a-b716-4d75-8b4a-7d2619f06b3a" />
<img width="2110" height="994" alt="image" src="https://github.com/user-attachments/assets/953a4fd7-38bd-418d-8aa9-2e134e1eca13" />


## Note
It is weird to me that using BPE in compression as it is unlike words merging in NLP, the relationship between neighboring frequency coefficients depends on location. And coefficients across actions could be merged as shown in the Fig which is more weird.
