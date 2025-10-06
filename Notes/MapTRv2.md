# [MapTRv2: An End-to-End Framework for Online Vectorized HD Map Construction](https://arxiv.org/abs/2308.05736)

This is the following work based on MapTR. Several modifications in architectures.
<img width="1899" height="1569" alt="image" src="https://github.com/user-attachments/assets/68fe3b16-f71d-4212-9cd1-2d8c2545c391" />
1. Rather than initial the complete query embedding, it initials instance-level query embeddings and point-level embeddings and form final query embedding by broadcasting add.
2. In self-attention, it applies self-attention on instance-level first and then point-level, which will reduce computation cost compared to apply self attention to all instances' points at the same time.
3. For cross-attention, it applies cross-attention to BEV features and images features individually and independently.
4. It considered complicated permutation situation in polylines and polygons.
   <img width="1979" height="1119" alt="image" src="https://github.com/user-attachments/assets/1d42e168-de6e-435e-a809-18bf3784dac9" />
   Polylines have two different permutations as it has two directions. Polygons have 2N different permutations where N is how many points one polygon has by setting different points are start/end point and set two different directions.
5. It has edge direction loss in Hungarian cost and final loss to calculate cosine similarity between edge directions.
6. one-to-many training config. One-to-many matching branch shares the same point queries and Transformer decoder with one-to-one matching branch, but own an extra group of T instance queries. We repeat the ground-truth map elements for
    K times and pad them with ∅ to form a set with size T. In the one-to-many matching branch, one GT element is assigned to K predicted elements. With the ratio of positive samples increasing, the map decoder converges faster.
7. It has three different auxiliary loss: Depth Prediction Loss, BEV Segmentation Loss (predicts foreground), and Image Segmentation Loss (predicts foreground).
