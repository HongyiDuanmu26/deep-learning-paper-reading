# [TopoMLP: A Simple yet Strong Pipeline for Driving Topology Reasoning](https://arxiv.org/abs/2310.06753)

The main point emphasized in this paper is: when the detection side is strong enough, topology (connectivity matrix prediction) can be treated as easily as using MLPs.

<img width="1636" height="988" alt="image" src="https://github.com/user-attachments/assets/63d46e9a-c54e-41f5-83ee-fac821ee5c7b" />

1. They used PETR as center line detector. It predicts the control points of the Bezier curve of center lines. Control points will be further processed into real coordinates.
2. YOLOv8 is used for traffic light detection, but in a query-like way.  YOLOv8 takes image features as input and generates multiple proposals, which are concatenated with a set of reference boxes produced from randomized queries,
   denoted as RT. The generated boxes by YOLOv8 are encoded by sine-cosine embedding to generate query features, which are concatenated with the randomized queries,
   denoted as QT. The query features as well as the reference boxes are fed into the deformable decoder.
3. Inside lane-lane topology prediction, we implement MLP to embed the lane coordinates and then add them into the decoded lane query features. Lane queries will be pair-wisely concatenated with each other to predict the final connectivity matrix.
   (It is weird to embed the coordinates again, as queries embedding should have position information already, though it is in Bezier control point space.)
