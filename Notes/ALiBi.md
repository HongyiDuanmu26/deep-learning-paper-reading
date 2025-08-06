# [TRAIN SHORT, TEST LONG: ATTENTION WITH LINEAR BIASES ENABLES INPUT LENGTH EXTRAPOLATION](https://arxiv.org/pdf/2108.12409)

common positional embedding types:
1. Absolute positional embedding. sin/cos based or learnable parameter based. It will be used only once for the first time before self attention.
2. Rotary position embeddings. RoPE. It will be used for all self attention layers not only the first one. I will be used only for $Key$ and $Query$ vectors and not be used for $Value$ vector.
3. Relative position embedding. In this method, we compute the attention values as before, but then we add a learned, shared bias to each query-key score that is dependent on just
the distance between the query and key. There is usually a maximum distance value used to fix the learnable parameter size.

In terms of extrapolation, the ability of training in short sequences and inferencing in long sequences, the authors have results as below.
<img width="1618" height="672" alt="image" src="https://github.com/user-attachments/assets/b5ced6d0-8d84-4656-9554-e16d9fa10bcd" />


The authors proposed "ALiBi" method, a relative position embedding as well. They add pre-fixed scales to attention scores based on distance as below. Also, it has no max distance limitation.

<img width="830" height="394" alt="image" src="https://github.com/user-attachments/assets/4759595b-7cc8-45e0-a657-253ef3d987ae" />
<img width="1624" height="404" alt="image" src="https://github.com/user-attachments/assets/337820ca-1542-46f1-b829-8e621d1deb68" />

Also, they used absolute positional embedding for each self-attention layer as well ($query$ and $key$ but not $value$), but did not mention a specific type.

## Note
Though it looks efficient as shown, the equation is too ugly to me.
