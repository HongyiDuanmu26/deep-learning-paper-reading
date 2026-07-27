# [Is Ego Status All You Need for Open-Loop End-to-End Autonomous Driving?](https://arxiv.org/abs/2312.03031)

Among end-to-end models, there is one phenomenon that systems based solely on ego status (Ego-MLP) can still get leading results in NuSence Challenge dataset. Also even masking perception inputs in some systems, little effects will have on the final performance. 
So, authors argue that: 1. NuSence dataset is easy as 74% data goes straight forward, which means keep ego status (velocity, direction) the same will have good performance. 2. current metrics (L2 and collision) cannot capture realistic performance as some trajectories can go off-road.

Authors built a naive BEV-planner to do some ablation studies. Here are some key results:
1. In UniAD, if we make image quality worse, little effect on final performance. But if we change ego status, a dramatic effect will be seen on final performance.  
<img width="2215" height="655" alt="image" src="https://github.com/user-attachments/assets/9dd36e73-fa9d-4ecd-9053-6133d7f5e3c1" />
2. If you add an auxiliary loss to recognize lanes, the system will be better in CCR (Curb collision rate) but could be worse in L2 and car collision rate.
   <img width="2233" height="277" alt="image" src="https://github.com/user-attachments/assets/41843dfe-edd2-458c-a432-406ec1c86607" />

