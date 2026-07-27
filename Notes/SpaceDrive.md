# [SpaceDrive: Infusing Spatial Awareness into VLM-based Autonomous Driving](https://arxiv.org/abs/2512.10719)

The authors argue that LLM in general not designed with capability in processing 3D information; thus, it would be hard for them to do 3D reasoning when dealing with autonomous driving problems. Also, treating numbers as digits and processed the same as text tokens makes it limited in numerical prediction.
So they proposed SpaceDrive.

<img width="2032" height="1354" alt="image" src="https://github.com/user-attachments/assets/c13827ad-b192-44a2-bab5-027c8d0049e3" />

1. Vision Encoding. After vision encoder, when projector to LLM space, they did not use Q-Former because 1. they think Q-Former could not fetch all 3D infos explicitly injected into the system. 2. harder to train with limited samples (they make SpaceDrive more like a plugin with existing LLMs). So a one-layer MLP is used as projector.
2. Spatial Encoding. A frozen depth estimator was used. Inside each image patch, the minimal depth among all pixels inside the patch is used as the depth of the patch. Depth along with the center of the image patch was encoded with sin-cos positional embedding.
3. Spatial Token Injection. 3D PE was element-wise added to features after MLP projector, making 3D infos more explicitly injected into LLMs. It is worth noting that direct additive injection of ϕ(cp) shifts the token norm distribution away from the pretrained VLM regime.
   To mitigate this, we introduce a learnable normalization factor αP E shared across all 3D PEs, simply: <img width="829" height="84" alt="image" src="https://github.com/user-attachments/assets/2e77b957-d508-4cad-b882-c41307081c38" />
4. Encoding of Coordinates in Text Prompts. Coordinates in text prompts will be encoded with sin-cos methods as well and a special token will be inserted before the coordinates (no after) <IND>. If it is a 2D coordinate, the z-axis will be set to 0.
5. Encoding of the Ego Status. Historical ego trajectory will be processed as coordinates as well.
6. Decoding of Text with Coordinates. In inference results, if a special token < IND> is found, the next will be a coordinate; the next one will be fed into the coordinate decoder, a fully learnable MLP. It is trained with Huber Loss for regression. 


## Notes
1. Only coordinates are specially processed. Other numbers in the system, like speed limit numbers, are treated as before like text tokens.
2. It seems like in decoding, only 2D coordinates are used.
