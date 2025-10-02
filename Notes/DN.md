# [DN-DETR: Accelerate DETR Training by Introducing Query DeNoising](https://arxiv.org/abs/2203.01305)

The authors believe the bottleneck of the training efficiency of DETR-based methods is Hungarian matching, as in the early stage, the matching results will be unstable, and 
for the same image, a query is often matched with different objects in different epochs, which makes optimization ambiguous and inconstant. So they proposed a denoising training technique.
Manually generated noised-GT objects as initial prediction and let the system learn the offsets of them, and this prior knowledge makes the training in the early stage easier. 

<img width="1896" height="1546" alt="image" src="https://github.com/user-attachments/assets/e1b08c35-d0a8-405b-ab9f-7b094d59937e" />

1. There are two main parts in the DETR: the matching part and the denoising part. Matching part is the same as the traditional DETR part, and the denoising part is the new one, and its inputs are
   noised-GT
2. GT will be added with random noise: 1) bounding box center shift, 2) bounding box width/height scale, 3) GT label flip (randomly switch to other class).
3. There are two types of potential information leakage. One is that the matching part may see the noised GT objects and easily predict GT objects. The other is that one noised
   version of a GT object may see another version. Therefore, our attention mask is to make sure the matching part cannot see the denoising part, and the denoising groups
   cannot see each other.
4. Vanilla DETR differs from DAB-DETR in that its positional queries are high-dimensional vectors without explicit meanings. For DN-Vanilla-DETR, we can simply use linear box
   embedding to embed noised boxes into the same dimension as DETR queries
