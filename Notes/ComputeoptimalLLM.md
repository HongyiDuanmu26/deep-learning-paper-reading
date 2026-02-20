# [Training Compute-Optimal Large Language Models](https://arxiv.org/abs/2203.15556)

A scaling law basic paper, arguing that most large models are undertrained due to insufficient training data. They trained more than 400 model runs to conclude that, given a fix 
training budget (model complexity and training steps), for compute-optimal training, the model size and the number of training tokens should be scaled equally.

The optimal model they provided is a 70B-parameter model trained with 1.4T tokens (not exactly as provided in the optimal table, because they want to compare to Gopher, a existing model, with the same computational cost). Roughly, $`D=20*N`$ and $`C=6*N*D`$, where D is the dataset size (number of tokens), 
N is the model size (number of parameters), and C is the computation cost (FLOPs).
<img width="1652" height="786" alt="image" src="https://github.com/user-attachments/assets/27a85cf3-1b77-450e-93a0-05549589ebab" />



## Appendix
1. In the cosine scheduler, x10 decay is about the same as/slightly better than decaying to zero; x10 decay is much better than x5 decay; We find that setting the cosine
cycle length too much longer than the target number of training steps results in sub-optimally trained models.
2. AdamW is better than Adam.
   
