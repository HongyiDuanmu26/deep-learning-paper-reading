# [Lane Graph as Path: Continuity-preserving Path-wise Modeling for Online Lane Graph Construction](https://arxiv.org/abs/2303.08815)

The main point of this paper is: comparing to breaking all centerline/lane boundary/road boundary into pieces at junction points and predicting piece instances with the connectivity matrix, 
why not predict the whole complete path instance directly? 

<img width="1574" height="870" alt="image" src="https://github.com/user-attachments/assets/3d44c2f6-b13f-4500-8355-c4cef6515214" />

Some minor points that need to be mentioned are:
1. One auxiliary loss in BEV segmentation.
2. The algorithm to retrieve the path from the connectivity matrix is: find all leaves whose out degree is 0, and find all roots whose in degree is 0. Then find path from all roots to all leaves.
Except for the points above, nothing else is worth mentioning.

## Notes
There are many drawbacks I can imagine in this design.
1. If the system needs to predict the centerlines within 100 meters, and the ego vehicle is currently driving on a long straight road with only one centerline, then in the next frame, a fork appears at 99 meters ahead.
This means that within the range from 0 to 99 meters under the ego vehicle, there will suddenly be two centerlines instead of one. Moreover, since the fork is far away, it’s very likely not visible in the camera view.
Such a sudden change in the ground truth makes it difficult for the system to learn effectively.
2. In post-processing, paths will first be cut into vertices (small pieces) and then extracted connectivity. Followed by a step to merge on close vertices, which would be highly influenced by cutting results.
3. A good academic way of thinking, but not that reasonable in industrial projects.
