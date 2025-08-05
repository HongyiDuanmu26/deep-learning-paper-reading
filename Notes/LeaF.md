# [Learning to Focus: Causal Attention Distillation via Gradient-Guided Token Pruning](https://www.arxiv.org/pdf/2506.07851)
The authors argue that in complex NLP tasks (such as math or coding), it is crucial to identify less important/misleading tokens. The performance will be boosted if input
tokens are well trimmed.

This is done by:
Taking a teacher model who can perform complex tasks well and a student model who did badly, comparing the difference between the gradients of loss to tokens from the teacher model and the student model.
If a few differ much, the corresponding tokens are attended incorrectly in the student model. 
<img width="1742" height="1134" alt="image" src="https://github.com/user-attachments/assets/314efe25-82c6-4327-843d-aa4a8d9f1d49" />

Then, align the output distribution between the teacher and the student model using the KL divergence on original input tokens and trimmed input tokens.
<img width="1744" height="688" alt="image" src="https://github.com/user-attachments/assets/f925ca70-61bc-469f-95ec-70cd6d47f9a5" />

## Notes
1. I have not found the hyperparameter information on the $\lambda$ used to balance the two KL divergences.
2. The authors mentioned that the threshold for token trimming is 0.05 - 0.1. It will be interesting to see how many tokens on average are trimmed.
   What if the sequence identified by the threshold is broken?
