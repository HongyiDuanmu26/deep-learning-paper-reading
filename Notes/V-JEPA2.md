# [V-JEPA 2: Self-Supervised Video Models Enable Understanding, Prediction and Planning](https://arxiv.org/abs/2506.09985)

Meta World model paper, a follow-up work of the first version, [V-JEPA](https://arxiv.org/abs/2404.08471).

The authors proposed a physical world understanding system. While training a pre-training base encoder, V-JEPA2, a ViT based model, by plug in different heads, it can solve different tasks.
<img width="1951" height="1321" alt="image" src="https://github.com/user-attachments/assets/d88ef536-623d-444c-881a-e1e92c50cbe0" />

Compared to the V-JEPA first version, the main difference between the two is that this one is larger.
1. Data scaling: We increase the dataset size from 2 million to 22 million videos by leveraging and curating additional data sources.
2. Model scaling: We scale the encoder architecture from 300 million to over 1 billion parameters, going from a ViT-L to a ViT-g.
3. Longer training: Adopting a warmup-constant-decay learning rate schedule simplifies hyperparameter tuning and enables us to extend training from 90 thousand up to 252 thousand iterations, effectively leveraging the additional data.
4. Higher resolution: We leverage the warmup-constant-decay schedule to efficiently scale to higher resolution video and longer video clips by training on shorter, lower-resolution clips during the warmup and constant phases, and then increasing resolution and/or clip-length during the final decay phase.

The basic method to pre-train the V-JEPA2 is the same as V-JEPA. By masking out internet-scale video data, the model was asked to reconstruct the original image in the latent space. The target encoder is EMA-version of the training encoder ($\theta_{EMA} = m * \theta_{EMA} + (1-m) * \theta$, EMA-encoder is the training one's historical smoothing)
Encoder is one ViT and predictor is one ViT. In this self-supervised way, authors want to have one physical understanding universal encoder. 
<img width="2032" height="1468" alt="image" src="https://github.com/user-attachments/assets/82f169e6-2353-4d5b-90e0-861f5d32909e" />

After pre-training is done, we have a pre-trained vision encoder. By plus in a 300M-parameter transformer model, it forms V-JEPA2-AC to predict next frame of video given robotics actions. States are 7 scalers, three for 2D position, three for diretion and one for state of the gripper. Actions are difference between states.
<img width="1899" height="1396" alt="image" src="https://github.com/user-attachments/assets/c232d428-4b3a-4913-8345-5ed2a22d38f9" />

They have two loss terms. One is called teacker-forcing. Predictor, given previous actions and states, vision features from encoder, was asked to predict the vision feature of next frame. 

<img width="1590" height="138" alt="image" src="https://github.com/user-attachments/assets/8f116fd5-8d76-4922-8cb1-5f1ec75540cf" />

The second loss is called roll-out loss, which will compare the GT with recurrent predicts for T timestemp from predictor. In practice, T=2. 

<img width="765" height="87" alt="image" src="https://github.com/user-attachments/assets/7e7a1aa5-9d92-4234-a01c-72817384d7e9" />

During inference, while you already have a goal image (image capturing the states you want the robots to be), search for a sequence of actions to minimize the difference between the imagined vision feature in T timestemps future and your goal vision feature in latent space.
Specifically, in each planning step, we sample the action coordinates at each point in the planning horizon from a sequence of Gaussian distributions initialized with zero mean and unit variance. The population statistics of the top-k actions trajectories
are used to update the mean and variance of the Gaussian distributions. This process is repeated for several iterations before finally returning the mean of the sequence of Gaussians as the selected action trajectory.

<img width="844" height="103" alt="image" src="https://github.com/user-attachments/assets/bc6be0fc-e2ec-4556-8ba5-a394f669c9f2" />



## Note
When dealing with higher input resolution: "Our training process begins with a warmup phase where we train on 16-frame, 256 × 256-resolution videos with linear learning rate warmup over 12K iterations, followed by a main training
phase with a constant learning rate for 228K iterations. Then, during the cooldown phase, we increase video duration and resolution while linearly decaying the learning rate over 12K iterations. Hence the additional computational overhead associated with training on longer-duration, higher-resolution videos is only incurred
during the final cooldown phase." 16x256x256 -> 64x384x384 and training in high resolution only hapends in the last 12k iterations while totally have 252k interations.
