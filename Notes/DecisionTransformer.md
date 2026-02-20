# [Decision Transformer: Reinforcement Learning via Sequence Modeling](https://arxiv.org/abs/2106.01345)

<img width="1432" height="852" alt="image" src="https://github.com/user-attachments/assets/8a5df922-1c3a-4fc3-9df5-d94ac50ab3f7" />
The Authors take the RL problem/behavior cloning as a sequencing modeling problem. R_bar is return-to-go. 
<img width="216" height="58" alt="image" src="https://github.com/user-attachments/assets/dba6c665-62d9-43d5-8b86-d92d0b6288ee" />

Given the sequence of all historical states, actions, and return-to-go, using a transformer-like model to predict the next action. 
In inference, it is used in an auto-regressive manner. By initializing a target reward, initial state, predict the first action. Update the return-to-go (target reward - first reward).
Feed updated state/action/return-to-go pairs into the transformer again for a new action.

## notes
* return-to-go acts like a prompt to ask model "if I want to get that amount of score, what should I do?"
* timestemp-wise positional embedding is added to model the timestemp.
* extreme better performance in long-sequence scenarios, where rewards can be affected by a far-ago action, compared to conventional Bellman backup methods.
  <img width="2200" height="470" alt="image" src="https://github.com/user-attachments/assets/10a0a3cd-a8e2-4390-82ab-117900c95935" />
