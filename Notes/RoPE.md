# [RoFormer: Enhanced Transformer with Rotary Position Embedding](https://arxiv.org/pdf/2104.09864)
One interesting paper on Positional Embedding was written by Jianlin Su from Zhiyi Tech.

In the self-attention mechanism, the input token is permutation-invariant. To make the self-attention learn the sequence information/order information, the positional embedding is proposed. 
There are mainly two ways of positional embedding: absolute positional embedding, which is mostly used with the temperature function, and relative positional embedding, which encodes the distance between tokens 
to align with the intuition that attention/similarity/connection between tokens decay with growing distance. 

To unify the two different positional embeddings, the main goal of this paper is to find a function to meet the criteria below:

$$
\langle f_q(x_m, m), f_k(x_n,n)  \rangle = g(x_m, x_n, m - n)
$$

where $x_m$ is the $m-th$ token of input $x$. The function shows two characteristics: 1. linear projection tasks the original input vector and position information; 
2. The inner product of the projected $q$ and $k$ is related to the relative distance $m-n$.

The proposed function is:
<img width="1342" height="494" alt="image" src="https://github.com/user-attachments/assets/6aace759-9ebc-44df-b373-04d4023ee9b0" />


## Notes
1. In computer vision tasks, we usually use learnable parameters as positional embedding, or we have explicit physical positions for all tokens (reference point for object queries or 3D physical position for image features).
   Also, it is weird to consider relative position in cross attention as indexing could mean different things in different sources of input. 
2. This positional method is used in VLA models like DeepSeek, LLAMA, and Qwen.
