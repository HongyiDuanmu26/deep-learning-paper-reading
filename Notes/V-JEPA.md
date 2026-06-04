# [Revisiting Feature Prediction for Learning Visual Representations from Video](https://arxiv.org/abs/2404.08471)

The basic architecture of this work is JEPA (Joint-Embedding Predictive Architectures). This work is about training a JEPA in a video dataset to enable the JEPA to have better general performance and ability in many other subtasks.
<img width="1255" height="844" alt="image" src="https://github.com/user-attachments/assets/31c6e19e-61d9-44a4-ad80-be1081e62703" />

The training logic is simple. Z is necessary information for data x to be transformed into data y. So we want to train an encoder E, which can capture necessary information in the latent space, and a predictor P, which can transform latent x into latent y given z. 
Loss is designed as the difference between the predicted y and the true y in latent space.

<img width="979" height="118" alt="image" src="https://github.com/user-attachments/assets/f479ddb9-4c67-4982-9835-a88806a0e10b" />

To prevent model training collapse, for example, the encoder outputs constants, the loss was modified as follows. where sg(·) denotes a stop-gradient operation, which does not backpropagate through its argument, and E_bar θ (·) is an exponential moving average of the network Eθ(·).

<img width="1030" height="126" alt="image" src="https://github.com/user-attachments/assets/e3475b8f-806c-41e9-b8f0-e49dec3b290c" />

The encoder that the authors used is ViT, and the predictor authors used is a narrow transformer implemented using 12 blocks with an embedding dimension of 384. The V-JEPA is trained by: mask-out tokens in inut x, let the predictor to reconstruct mask-out tokens. 

<img width="2533" height="1153" alt="image" src="https://github.com/user-attachments/assets/da399acd-794e-42f0-859a-39e6c251cc55" />

In section 4.1, the author's experiments show that reconstruction in latent space is better than reconstruction in pixel space. 

In section 4.2, the author's experiments show that a larger dataset helps in general, but for a specific task, it would be better if seen more in that task category.

In section 4.3, the author's experiments show that while you can get vision features by average pooling for downstream tasks, it is better if you use a learnable query and cross attention to fetch features. 
