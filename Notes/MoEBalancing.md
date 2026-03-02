# [Auxiliary-Loss-Free Load Balancing Strategy for Mixture-of-Experts](https://arxiv.org/abs/2408.15664)

MoE models usually use loss functions to control the loading balancing problem, while we want to make sure experts are activated evenly. However, adding a new load-balancing term 
to the loss function may affect model performance as you are asking the model to optimize two goals at the same time with one set of parameters. 

So authors proposed one balancing strategy that is loss-free and has nothing to do with the original model parameters so that model performance will not be limited by load balancing.

<img width="1708" height="632" alt="image" src="https://github.com/user-attachments/assets/e1ddf2c0-1c1c-48af-b3ce-a9f2d46843a6" />

The routing part will have one more set of parameters $b_i$, which will be added to experts' scores. This set of $b$ is learnable and will be updated in each batch.
For each batch, it will calculate the loading of each experts, if it is above average, the corresponding $b$ will be decreased, and vice versa. So $b$ is determined by all historical loading information.

<img width="1732" height="1304" alt="image" src="https://github.com/user-attachments/assets/098530d3-1518-4b2d-9a06-d0830bb4fc86" />

So, the results showed that this way can help dramatically improve the balancing of MoE and slightly improve performance. 

## Note
1. This method is only used in training to make sure experts are balancedly trained.
2. A really good explaination of this paper by Jianlin Su [https://kexue.fm/archives/10757](https://kexue.fm/archives/10757).
