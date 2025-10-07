# [Continuity Preserving Online CenterLine Graph Learning](https://arxiv.org/abs/2407.11337)

A paper focusing on center line detection in autonomous driving. Mainly three new components.

<img width="2094" height="1204" alt="image" src="https://github.com/user-attachments/assets/7bb177fa-b9e6-4d90-bddc-bb742c07e31f" />

1. Junction Aware Query Enhancement Module. After getting BEV features, there is one more branch to segment key points in BEV space as an auxiliary loss.
   The query position embedding will cross-attend to the key-point-BEV-feature to capture information on key points. (Interestingly, in the codes,
   in cross-attention, key-point-BEV-feature is flattened as key and value, and no position embedding is used for it.)
2. There is one more auxiliary loss on Bézier curve. After getting all the Hungarian pairing results, you will know which two prediction polylines should be connected based on GT info.
   The two supposed to be connected predictions' raw lane embedding will be concatenated and do Bézier in the hidden space, and using an MLP to predict control points.
   The control points will be supervised with GT-paired two polylines' Bézier control points as an auxiliary loss. (Not sure why do it in hidden sapce rather than final 2D BEV space)
3. To output connectivity, it implies a GNN-GRU module. The connectivity matrix is initialized with all zeros for GNN. The GNN-GRU module is repeated N times.
   The updated lane embedding will be pair-wise concatenated to predict connectivity by MLP. Supervision is deployed at all levels of output.

## Notes
1. The key point information fetching part is interesting, but it is weird to me that there is no position information in cross-attention.
   Though the system can learn something, suspicious to me.
2. In the ablation study, the Bézier part has more obvious improvement among the three components. Not sure if it is applicable for industrial projects. 
   
