# [FastDriveVLA: Efficient End-to-End Driving via Plug-and-Play Reconstruction-based Token Pruning](https://arxiv.org/pdf/2507.23318)
Vision tokens are widely used in driving-aimed LLMs. As vision tokens are usually redundant, pruning is deployed before being fed into LLMs. Authors argue two typical pruning methods are
less optimal: 1. pruning based on attention score between text tokens and vision tokens, which will be largely affected by the quality of texts. 2. pruning based on similarity between vision tokens 
to save only diverse tokens. It will easily attend to diverse but meaningless tokens, such as background.

A public/pretrained foreground segmentation model is used to classify foreground and background. 

A single decoder layer of Qwen2.5-VL-3B is used as a PrunerLayer. One later MLP is used as a scorer. High-score tokens are asked to reconstruct the foreground image and
Low-score tokens are asked to reconstruct the background. The reconstruction decoder consists of 6 Qwen2.5-VL-3B layers and a feedforward reconstruction head.
<img width="1870" height="1366" alt="image" src="https://github.com/user-attachments/assets/ca2014b0-0e81-42f2-bdfa-0f28e276516a" />

Mask operation is adopted by Straight-Through Estimator (STE) to make it differentiable. 

## Notes
1. A fixed pruning rate will be set, which means it limits the total meaningful vision tokens for frames.
