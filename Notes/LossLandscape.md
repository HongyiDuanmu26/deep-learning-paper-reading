# [Visualizing the Loss Landscape of Neural Nets](https://arxiv.org/pdf/1712.09913)
In this paper, authors want to viz all loss-

Given a center point of $\theta^{\star}$, trained model weights, randomly (Guassian) selected two directions which have the same shape as $\theta^{\star}$.
<img width="1661" height="252" alt="image" src="https://github.com/user-attachments/assets/3982b527-f1d1-4111-b59a-7cd2bd61b6c1" />

Two directions are normed per-filter based on the norm of each filter in $\theta^{\star}$. $d_{i,j}$ is j-th filter in i-th layer if direction. $\theta_{i,j}$ is j-th filter in i-th layer in trained model.
<img width="314" height="65" alt="image" src="https://github.com/user-attachments/assets/5dc579cb-f8f0-4741-9303-b7762b86dc20" />

Then you can plot loss 2D surface.

## results

1. the deeper the net is, more convexity there will be, especially if without skip-connection.
2. skip-connection can drametically help in convexity. 
<img width="1616" height="823" alt="image" src="https://github.com/user-attachments/assets/b159c4f8-b3b6-407b-af93-b7c345a4ad56" />

3. then wider the model is (larger channel size), the better in convexity.
<img width="1645" height="813" alt="image" src="https://github.com/user-attachments/assets/daff5b38-1b0c-44d8-a4ce-af23eb253feb" />

4. [Implication] When surface is too non-convex, the initial point of training would be more importent as gradient in more non-convex area is misleading preventing it from well training.

## Notes
1. The norm on directions is to laign norm of model paprameters, which means it makes comparison between models are differnt as model parameter norms are differnt. While scaling is different, (1, 0) means 1 unit distance (parameter norm) to original point is differnt.
   So it is hard to tell flatness and sharpness.
2. it will be interesting to see more different plots when drawing differnt random directions. Appendix 4.
