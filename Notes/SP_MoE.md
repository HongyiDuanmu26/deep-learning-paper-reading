# [OUTRAGEOUSLY LARGE NEURAL NETWORKS: THE SPARSELY-GATED MIXTURE-OF-EXPERTS LAYER](https://arxiv.org/pdf/1701.06538)

<img width="1860" height="874" alt="image" src="https://github.com/user-attachments/assets/082a764a-3828-46d5-b01d-61f36dd73d3c" />

The authors introduced one new Mixture of Experts (MoE) layer, one gating module, and several expert modules (usually the same but could be different, in this article, it is
linear layer/layers.)

The gating module can be a simple softmax function to make sure it is back-propagatable, but it is not that sparse to save computation. 

<img width="1734" height="474" alt="image" src="https://github.com/user-attachments/assets/8413a1f1-79dc-48bc-b256-431a56bc8366" />

So, the authors proposed this gating function. First, only k experts out of N experts are activated, so you do not have forward calculation in all other (N-k) experts, 
and much computation is saved while the model (# of parameters) can be large. Second, the noise term here is designed for diversity in expert activation, to avoid the situation where 
some experts are always activated, but some are never activated.

The authors observed that the gating network tends to converge to a state where it always produces large weights for the same few experts. They proposed two loss terms to control it.
<img width="1546" height="276" alt="image" src="https://github.com/user-attachments/assets/534344d3-1d00-4960-923a-fe325b68300a" />
This loss is large when the importance (sum of activation scores of that expert in this data  batch) is dominated in only some experts. It is small when importance is evenly distributed.

The second loss term is called load-balancing loss, to encourage experts to receive roughly equal numbers of training examples.
<img width="1764" height="1360" alt="image" src="https://github.com/user-attachments/assets/908884c1-75f6-4dde-ab6d-68b7469f2c7d" />



## Notes
Normally, in multi-GPU training, the same model parameters will be stored in all GPUs and forward/backward separately in all devices. But now, as the model has too many small experts,
it is hard to fit the whole model into the GPU. What the authors did is, devide experts into batches and put each batch into one GPU. And the data example will be fed into the corresponding device
based on which experts are activated. So, this can help in memory usage while you have too many experts, but it needs more communication cost. So generally, larger experts mean better efficiency.
