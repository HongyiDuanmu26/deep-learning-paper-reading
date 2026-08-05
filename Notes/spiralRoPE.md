# [Spiral RoPE: Rotate Your Rotary Positional Embeddings in the 2D Plane](https://arxiv.org/abs/2602.03227)

In 1D-RoPE, the channel dimension was divided into many 2D pairs and each pair will be asigned a rotation matrix, depending on token position and base frequency.
<img width="1207" height="778" alt="image" src="https://github.com/user-attachments/assets/8b9a0e80-1d7f-4f4b-b9d3-4d2f8f114bfd" />

There is one way of 2-dimensional RoPE, like used in QWen2. If there are two variabls to represent a position, just split all channel dimensions into two and apply 1D-RoPE to each. Frequency map is the same for x and y.
<img width="1236" height="600" alt="image" src="https://github.com/user-attachments/assets/75f51c8d-0eb1-4bdb-8075-c56d2e93643d" />

Authors argue that, in this way, all directional position shift will be projected into x and y axis only, thus call this method axial 2D RoPE. This will let the system pay only attention to x and y direction shift and lack of ability in representing diagnal direction shift, for example.
So they proposed to use a set of directions. We uniformly distribute K (for example, 4 for 8) directions in the angular range [0, π). The channel dimension was divided into K groups and each group is in charge of a direction.
<img width="1224" height="1108" alt="image" src="https://github.com/user-attachments/assets/ac890804-af17-40ee-9234-9a241ef75bbd" />

The total set of frequency is shown below, similar to axial RoPE. 
<img width="1045" height="111" alt="image" src="https://github.com/user-attachments/assets/4b21aefe-2048-4f2e-be91-f2bef7d1d85a" />
 
We first group adjacent frequencies into pairs: (θ0, θ1), (θ2, θ3), . . . , (θd/4−2, θd/4−1). Then frequency pairs are asigned to directions in a round-robin fashion. Directions that are 90° apart (i.e.,
perpendicular), since they encode orthogonal spatial relationships, will use the same set of frequency. For example, with K = 4 directions (0◦, 45◦, 90◦, 135◦)
and d = 32, the frequency assignment is:
<img width="1123" height="204" alt="image" src="https://github.com/user-attachments/assets/9869ad0a-c5da-42d0-bb26-6c768ecb5bd9" />

## Note
The interleave frequency asignment is similar to what did in QWen3-VL. They both try to evenly asign low/high frequency to different directions, as low frequency might be helpful in catching long distance infos and high frequency might be helpful in catching local infos.
