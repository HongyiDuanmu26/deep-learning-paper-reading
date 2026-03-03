# [DeepSeekMoE: Towards Ultimate Expert Specialization in Mixture-of-Experts Language Models](https://arxiv.org/abs/2401.06066)

The authors proposed two main changes in MoE:
1. Given N experts, activating K each time. By reducing the size of one single expert m times, with the same computation cost, you can have mN experts and activate mK experts each time.
Thus, they called it fine-grained experts. Experts can be more specified.
2. Given N experts, activating K each time. You can set S experts called "shared experts", which will always be activated. Normally, S << K - S. It acts like a shared knowledge learner
and all other acts like specified knowledge learner.

Load balancing loss have two terms, expert-level and device-level. "When aiming to alleviate computation bottlenecks, it becomes unnecessary
to enforce strict balance constraints at the expert level, because excessive constraints on load balance will compromise model performance.  In practice, we set a small
expert-level balance factor to mitigate the risk of routing collapse, and meanwhile set a larger device-level balance factor to promote balanced computation across the devices."

<img width="1966" height="1168" alt="image" src="https://github.com/user-attachments/assets/b1818e4e-126e-43e6-b17a-be55f2848bee" />

