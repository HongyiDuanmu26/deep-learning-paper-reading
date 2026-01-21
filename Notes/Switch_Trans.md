# [Switch Transformers: Scaling to Trillion Parameter Models with Simple and Efficient Sparsity](https://arxiv.org/abs/2101.03961)

A follow-up work in MoE (Mixture of Experts). 
1. Different from the previous MoE paper, [Shazeer 2017](https://arxiv.org/abs/1701.06538), which uses top-k method for experts routing,
the authors use only top-1, because of computing efficiency (though they claimed better performance).
2. Also, a new term, "capacity factor" is introduced to control 
the maximum number of tokens processed by each expert.

<img width="1926" height="1286" alt="image" src="https://github.com/user-attachments/assets/f72a8190-e140-4777-9811-9327cc02fc55" />

When capacity factor is set as 1, experts will take only (total tokens / num experts) tokens in each layers. When tokens are not evenly distributed, those tokens will be skipped in
this layer and pass this layer via the residual connection. It is introduced surely because of efficiency. 

3. A new balancing loss is introduced. Rather than using two balancing loss in [Shazeer 2017](https://arxiv.org/abs/1701.06538), they proposed a fused one. 

<img width="1908" height="952" alt="image" src="https://github.com/user-attachments/assets/e4e1230b-9add-4433-a36f-df87d0057c52" />

4. Because of top-1 routing, it is more difficult to maintain training stability. Several techniques applied.
  * set routing part high-precision training float32 and other parts bfloat16.
  * truncated normal distribution with mean $\mu = 0$ and standard deviation $\sigma = \sqrt{s/n}$ where s is a scale hyper-parameter and n is the number of input units in the weight tensor.
    Values greater than two standard deviations from the mean are resampled. As an additional remedy to the instability, we recommend reducing the default Transformer initialization scale s = 1.0 by a factor of 10. This both improves quality and reduces
    the likelihood of destabilized training in our experiments.
  * After pre-training, when fine-tune in a specific subtask with a smaller dataset, it is easier to overfit. They found a better way for it: drop-out ratio 0.1 for other layer
    and drop-out ratio 0.4 between linears in experts.

## Appendix
1. They tried to replace Q,K,V linear layer in self-attention to switch layers. Better performance but bad stability.
2. They tried to make sure all tokens being processed by one experts by degraded to the second/third highest routing experts. But worse performance.
3. To encourage exploration across experts (explore more even with a lower routing score), they tried 1) deterministic or argmax 2) sampling from the softmax distribution
   3) input dropout on the incoming representation 4) multiplicative jitter noise on the incoming representation. 2) is the worst, and 4) is the best.
      <img width="1564" height="568" alt="image" src="https://github.com/user-attachments/assets/46c6cb40-deaa-4bf3-97b5-4c00c9f746f1" />
