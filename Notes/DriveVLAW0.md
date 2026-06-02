# [DriveVLA-W0: World Models Amplify Data Scaling Law in Autonomous Driving](https://arxiv.org/abs/2510.12796)

In VLA models, the authors argue that the supervision on waypoints is too sparse and not enough and inefficient for such a big model. So they added world-model-like image/vision space supervision into it.

<img width="1809" height="1246" alt="image" src="https://github.com/user-attachments/assets/ee358ced-69b2-428d-80c9-d9039aadec83" />

Authors designed different supervision on two VLA types: VLA (VQ), which quantizes images into discrete visual tokens for an Emu3-style backbone, and VLA (ViT), which extracts continuous features for a Qwen2.5-VL-style backbone.

If it is VLA (VQ), the output vision features are tokenized. The new supervision is called AR-WM. The new design loss is on the next vision token prediction. Given all previous information, predict next token auto-regressively. As the attention mask is causal, no more computation cost is needed,
and there is only one more loss term, as shown below (S is the whole input prompt).
<img width="892" height="168" alt="image" src="https://github.com/user-attachments/assets/25a2595e-f21b-4c8e-8493-351acaa944d7" />

If it is VLA (ViT), the output vision features are continuous features. As the next token prediction is hard in a non-discrete manner, authors proposed a diffusion-based supervision. Diffusion model is a trained model. It is used to generate the camera images in the next time frame given
the output vision and action feature from VLM.

<img width="966" height="91" alt="image" src="https://github.com/user-attachments/assets/d56031f6-1d81-4d3a-9c4d-7ae747d849d8" />

Another highlight of this paper is: as the VLA model is usually big, they designed a small model with a similar architecture with only a smaller hidden size, called action expert. 
<img width="1819" height="709" alt="image" src="https://github.com/user-attachments/assets/b3620559-ee58-4a96-b9e8-0e692f2d988c" />
<img width="1143" height="109" alt="image" src="https://github.com/user-attachments/assets/0e9d571c-cb18-4cec-9671-8d084ede22a9" />
When doing attention, the QKV from VLM and action expert are concatenated and other operations remain separated. Sp VLM is forwarded once and no auto-regressive operation is on the large model. If needed, auto-regressive operation can run only on the small model, action expert.

