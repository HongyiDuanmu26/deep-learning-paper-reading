# [EMMA: End-to-End Multimodal Model for Autonomous Driving](https://arxiv.org/abs/2410.23262)

Waymo's work on using Gemini in end-to-end motion planning. 
<img width="1755" height="1044" alt="image" src="https://github.com/user-attachments/assets/8ce680b2-4ae1-4286-9175-0e2ee686b30a" />

Inputs are: 1) Surround-view camera videos (4 frames only); 2) High-level intent command, derived from the router, includes directives such as “go straight”, “turn left”, “turn right”, etc.; 
3) Set of historical ego status, represented as a set of waypoint coordinates in Bird’s Eye View (BEV) space, $T_{ego} = \\{(xt, yt)\\}^{−Th}_{t=−1}$ for $T_h$ timestamps. All waypoint coordinates are represented as plain text without specialized tokens.

<img width="628" height="94" alt="image" src="https://github.com/user-attachments/assets/c66c88dd-1b8b-4336-bddf-ae66688e67f7" />

By passing those one vision inputs and two text inputs into Gemini, it produces way points coordinates of motion planning represented as plain text.

It also incorporates CoT, asking the model four more questions before asking for the motion plan. 
1. Scene description broadly describes the driving scenarios, including weather, day of time, traffic situations, and road conditions.
2. Critical objects are the on-road agents that can potentially influence the driving behavior of the ego vehicle, and we require the model to identify their precise 3D/BEV coordinates.
3. Behavior description of critical objects describes the current status and intent for the identified critical objects.
4. Meta driving decision includes 12 categories of high-level driving decisions, summarizing the driving plan given the previous observations.

"We highlight that the driving rationale captions are generated using an automated tool without any additional human labels, ensuring scalability of the data generation pipeline. Specifically, we leverage off-the-shelf
perception and prediction expert models to identify critical agents, and then use Gemini models with carefully designed visual and text prompts to generate scene and agent behavior descriptions. Meta driving decisions are computed using a heuristic algorithm that analyzes the ego vehicle’s ground-truth trajectory."

During both training and inference, the model predicts all four components of the driving rationale before predicting the future waypoints. Empirically, we
observe that the prediction order of $O_{rationale}$ and $O_{trajectory}$ does not result in a significant difference in
quality after model convergence. This suggests that we can predict $O_{trajectory}$ first and apply early stopping
during inference for time-critical applications.

They trained the model with three more tasks:
1. Spatial reasoning/3D object detection. By asking Gemini "detect every object in 3D", it predicts $O_{boxes} = set\\{text(x, y, z, l, w, h, \theta, cls)\\}$. While $O_{boxes}$ is an unordered set of boxes, the predictions from an auto-regressive language model are always
ordered. We find that sorting the 3D bounding boxes by depth improves detection quality, unlike the findings
in Pix2Seq.
2. Road graph estimation. They convert lanes into sets of ordered waypoints and (b) transform these
sets of waypoints into text. Just like detection, we also find that ordering lanes by approximate distance improves the
prediction quality. An example of our polyline text encoding is: "(x1,y1 and... and xn,yn);..." where
"x,y" are floating point waypoints with precision to 2 decimal places, ";" separates polyline instances.
3. Scene understanding. text prompt "Is the road ahead temporarily blocked?"

## notes
no more details in model training configurations.
