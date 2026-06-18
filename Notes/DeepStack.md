# [DeepStack: Deeply Stacking Visual Tokens is Surprisingly Simple and Effective for LMMs](https://arxiv.org/pdf/2406.04334)

In LLms, when it is designed to process images/videos, it is always struggling to balance computational cost and input resolution. The authors proposed to feed image patches at different levels into the LLMs at different self-attention layers.
The fusion of different levels of vision information is done by adding.
<img width="1654" height="880" alt="image" src="https://github.com/user-attachments/assets/c1ed7739-bb08-4686-8130-f8dca4280acd" />
<img width="1638" height="601" alt="image" src="https://github.com/user-attachments/assets/e92b65c1-8901-4bdf-b286-e544b645fbfc" />


The authors conducted experiments in different settings. a, what if feed vision information in later layers, while initializing the corresponding input embeddings to zero. Inserting them beyond the midpoint leads
to a significant performance drop; b, authors fixed global visual tokens at the input layer and varied the interval between two decoder layers for stacking high-resolution tokens; c, increasing the layers for stacking consistently enhances overall performance,
with the best results achieved using four layers.
<img width="1629" height="628" alt="image" src="https://github.com/user-attachments/assets/7051b592-5b62-4413-bd54-a742ce8524b1" />

Authors tried some other sampling methods for image patch in different resolutions. The "2D spatial" way is the best as it makes image tokens in the same position focus on the same area of the original image. 
<img width="1669" height="1003" alt="image" src="https://github.com/user-attachments/assets/e6bf91d9-d78f-417b-9892-3d8d20cbb7e7" />
