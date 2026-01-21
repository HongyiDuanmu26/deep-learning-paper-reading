# [DeepSpeed-MoE: Advancing Mixture-of-Experts Inference and Training to Power Next-Generation AI Scale](https://arxiv.org/pdf/2201.05596)

It is known that MoE based models can achieve better performance with no more requirements in inference latency because part of the model is activated at each time, 
though with more requirements in memory usage.
<img width="1444" height="648" alt="image" src="https://github.com/user-attachments/assets/5f2d37fa-20ae-4cde-8071-acf2539f82e6" />

1.3B+MoE-128 model can be as good as a 6.7B dense model but actually 1.3B+MoE-128 has 52B parameters. So another problem for MoE model is "parameter efficiency".

<img width="1472" height="654" alt="image" src="https://github.com/user-attachments/assets/70114343-9946-4098-b8ce-7cbfc8cd50d6" />
The authors proposed PR-MoE (Pyramid-Residual-MoE) based on two observations:

* Second-half MoE (using MoEs only in the second half of the model) is better than first-half MoE (using MoEs only in the first half of the model).
   Deeper layers benefit more from a large number of experts.
* Second, to improve the generalization performance of MoE models, there are two common methods: (1) increasing the number of experts while keeping the expert
  capacity (aka for each token, the number of experts it goes through) to be the same; (2) doubling the expert capacity at the expense of slightly more computation (33%)
  while keeping the same number of experts. The authors compared 1) Top2-MoE and 2) Residual-MoE (a token will always pass a dense MLP module and an Top-1 gating MoE module,
  to reduce the communication cost in Top-2 MoE). These two have similar performance.

So the PR-MoE (Pyramid-Residual-MoE) is: 1) using MoE with fewer experts in shallow layers and using MoE with more experts in deeper layers. It is called "Pyramid", and 
2) change MoE to Residual-MoE (a dense MLP + Top-1 MoE).

They also proposed MoS (mixture-of_students), MoE with fewer channels. MoS is initialized with pretrained MoE and KD (knowledge distillation) loss. 
<img width="608" height="92" alt="image" src="https://github.com/user-attachments/assets/b26e5cdf-ec55-4e52-8720-9cb91d3822d5" />


 
