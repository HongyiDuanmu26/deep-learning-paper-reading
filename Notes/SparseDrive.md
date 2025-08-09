# [SparseDrive: End-to-End Autonomous Driving via Sparse Scene Representation](https://arxiv.org/pdf/2405.19620)
A end-to-end model for motion prediction and planning at the same time, close to UniAD

It mainly consists a symmetric sparse perception module (vectorized no dense BEV; "symmetric" probably means similar share architecture for static object detection and dynamic object detection),
a Parallel Motion Planner ("Parallel" probably means motion prediction and motion planning at the same time)
<img width="2117" height="1335" alt="image" src="https://github.com/user-attachments/assets/8c4bb83a-1f54-4cbb-a38d-3890d8174f76" />

static/dynamic objects detection share similar architecture. Each consists one single non-temporal decoder block and 5 temporal decoder block. Each initialized query has corresponding anchors, including physical information of objects,
location, dimension, yaw angle and velocity fort dynamic objects and x, y coordinates for static objects. Fixed with K-means in training set.
In each decoder block, it will output offsets of anchors to refine it for the next layer.
<img width="2078" height="1168" alt="image" src="https://github.com/user-attachments/assets/407ba36f-23dc-4761-9e86-c49fab5aaa3d" />

Ego instance query is initialed with front camera feature map and will be concatenated with all dynamic instance queries to form agent queries. Agent queries will be cross-attented with historical agent queries, self-attended, cross-attened with static objects.
Module will produce trajectories and scores. Scores will be modified further based on collision risk of each planning trajectory proposal. For the trajectory with high collision probability, we reduce the score of this trajectory. 
In practice, we simply set the score of collided trajectory to 0. Finally, we select the trajectory with the highest score as the final planning output.
<img width="2060" height="1033" alt="image" src="https://github.com/user-attachments/assets/a4627f74-cf82-4d2b-ae1f-55f4b4ee1921" />


## Note
1. Rather than raondom initialization, ego car instance query is initialed with front camera feature map.
2. As dynamic queries will be concatenated with ego instance query. Will ego instance query be concatenated with itself to form the self agent query?
3. Not sure if aspatial transformation will be used for historical queries. For queries or for anchors?
4. Cannot find details on collision-aware rescore. Probably need to check codes. 
