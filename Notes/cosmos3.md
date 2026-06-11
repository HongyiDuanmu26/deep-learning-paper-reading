# [Cosmos 3: Omnimodal World Models for Physical AI](https://research.nvidia.com/labs/cosmos-lab/cosmos3/technical-report.pdf)

## Architecture related details

Cosmos3 is designed to have the ability to process multiple sources of input, including text, image/video, and actions, and outputs text, image/video, actions, and audio. 

<img width="1909" height="945" alt="image" src="https://github.com/user-attachments/assets/ea033a93-21e9-427d-894a-24a77ebe74ae" />

Different modalities of inputs need different encoders. Image/Video has one ViT encoder for understanding and one VAE encoder for generating. Audio has a VAE encoder as it is only appears in outputs of generative tasks. No encoder needed for texts.
For different actions in different tasks, they have different DoF and are designed with different dimensions of vectors. All actions will be projected to the latent space with one linear layer with different parameters for different types of actions.

<img width="1921" height="1285" alt="image" src="https://github.com/user-attachments/assets/36158fa8-1a6f-4414-ab4d-0206ac536c16" />

The authors proposed one schema that the input sentence consists of two parts, one is auto-regressive subsequence and a diffusion subsequence. Auto-regressive subsequence contains language tokens as well
as video and image tokens embedded by the ViT encoder. Diffusion subsequence follows the AR subsequence and contains video and image tokens from the VAE
encoder, as well as audio and action tokens. For any given task, we apply the same format to arrange these tokens: (1) autoregressive tokens are placed
before diffusion tokens; (2) within the diffusion subsequence, for each modality, clean conditioning tokens are
placed before noisy diffusion tokens; and (3) within both the conditioning and diffusion subsequence, tokens
are ordered by vision, audio, and action modality. We denote clean vision, audio,
and action tokens as 𝑣, 𝑠, and 𝑎, respectively, and their noisy counterparts with tildes: ˜𝑣, ˜𝑠, and ˜𝑎. 

So for different tasks, input tokens are arranged like:
1. Language. only autoregressive subsequence. Vision (optional) and text tokens are fed into the system acting like a VLM.
2. Text-to-Image. autoregressive subsequence contains the language tokens, while the diffusion subsequence contains the noisy target image tokens embedded by the VAE encoder.
   <img width="1950" height="61" alt="image" src="https://github.com/user-attachments/assets/30226091-07b5-47cb-9025-7121bfa49fd9" />
3. Text-to-Video (+Audio). Similar to Text-to-Image, but the diffusion subsequence contains the noisy target video tokens instead.
   <img width="1867" height="136" alt="image" src="https://github.com/user-attachments/assets/8ddfc584-bf0e-4519-819b-118a84d8e759" />
4. Image-to-Video/Video-to-Video (+Audio). In the diffusion subsequence, the clean conditioning image or video tokens are followed by the noisy target video tokens.
   <img width="1807" height="76" alt="image" src="https://github.com/user-attachments/assets/e43f73b9-042f-4482-b847-664590a96598" />
5. Video transfer. Similar to that of Video-to-Video, with the control-video tokens used as conditioning tokens and the RGB video tokens used as noisy target tokens.
   <img width="1846" height="91" alt="image" src="https://github.com/user-attachments/assets/ae23db09-3453-46de-89a7-90448b4ed3f9" />
6. Action.
   <img width="1896" height="733" alt="image" src="https://github.com/user-attachments/assets/1baa2887-b990-4ffc-ba6c-c9c271942450" />

When the sequences are formed, they are fed into a MoT (Mixture-of-Transformers). Two parallel transformer blocks have interaction in the self-attention layer.
<img width="1951" height="1593" alt="image" src="https://github.com/user-attachments/assets/3a0f75ec-cfdd-49b3-8309-3dec7dff3d82" />

The auto-regressive part is attended within itself with a causal mask, and the diffusion part can attend to all tokens.
<img width="1726" height="97" alt="image" src="https://github.com/user-attachments/assets/7bb941bb-cf38-49a9-b766-54b8d9f298bd" />
<img width="1690" height="115" alt="image" src="https://github.com/user-attachments/assets/05278e54-f045-4f8a-9aa0-cb6c449b001b" />

One challenge for positional embedding is that video, audio, and actions could be sampled asynchronously and at different sample rates. There should be one time positional embedding based on real physical timestemp rather than simply indexing it. 
For language tokens, 𝑡 = ℎ = 𝑤 is set to the same monotonically increasing value. For vision tokens, temporal indexing is shared within one frame. All audio tokens and action tokens only carry temporal coordinates. The spatial indices are set
to zero (ℎ = 𝑤 = 0). All tokens in diffusion parts will share the same temporal offset, depending on how many temporal indexs used in auto-regressive parts. "In practice, we find that directly letting the diffusion tokens start
from the temporal offset of the last autoregressive token leads to over-saturation and checkerboard artifacts in the initial video frames." To address this issue, we insert a fixed temporal gap between the autoregressive and diffusion
subsequences, uniformly shifting the temporal indices of all the subsequent vision, audio, and action tokens, and the fixed temporal gap was set to be 15000. To deal with different sampling rate problem, the temporal positional embedding is normed by 
how long the timeframe one token represents. For example, vision tokens from 24Hz video means 6Hz as VAE will compress video 4 times in temporal axis. Audio tokens in 48 kHz, 1920 hop size, means 25Hz. For action tokens, it is the sampling frequency of the action data.
<img width="1704" height="732" alt="image" src="https://github.com/user-attachments/assets/1d21ee18-52d7-45c8-afa1-885567831876" />

## data
The data part is divided into reasonor (auto-regressive) part data and generator (diffusion) part data.
Reasonor data:
1. 22M pre-training data, focusing on image-text and text-text data for general understanding ability. 2.2M finetuning data focusing more on Physical AI specialization with more video–text samples from robotics, smart infrastructure, and autonomous
vehicle domains.
2. Large pre-training dataset was screened by semantic deduplication. Embeddings of training samples are generated by Qwen3-VL-Embedding-8 and PE-Core-G14-448 model. K-means clustering is applied, and within each cluster, duplicate samples are identified and removed based on cosine similarity.
3. Gemma-4 is used to score the training sample in three quality dimensions: Faithfulness, whether response claims are grounded in the provided image, video, or textual context; Completeness: whether the response fully addresses the instruction without important omissions; Correctness: whether the response is factually, logically, and task-level accurate. The model judge also produces short evidence-based rationales enabling targeted spot checks and auditing of the filtering behavior. The filtering rule is, data sample needs to be above one specific threshold in all three categories. And authors experienced some dramatic fall in performance when increasing threshold from 2 to 5, like referring-expression, image captioning, and visual question answering. So they used 2 in pre-training and 5 in SFT (Supervised FineTuning).
4. In SFT, focusing on Physical AI specialization, they added some specific datasets to enhance General spatial understanding, General temporal understanding, Autonomous vehicle, Robotics and embodied AI, and Smart infrastructure. 

Generator data:
1. For Image and Video data, in pre-training, authors collect raw data, check raw data quality, generate embedding and deduplication, and categorize data and filter them based on aesthetic and photorealism scores get from VLM model. In mid-training, data includes more synthetic and text-rendering images and manually created training data for specific tasks, like given depth map, generating RGB images in specific domains like AV and robots. Authors claim Caption quality is a critical factor in generation quality. Rather than using free-form natural-language captions, they used a json like format to fulfill all aspects of the inputs, including objects, attributes, relationships, and scene-level information, generated by Qwen3-VL. And LLM and VLM used to evaluate the Precision (whether each claim is visually supported by the image or video itself) and Recall (comprehensiveness).
2. For audio, in pre-training, model will be fed with a broad mixture of sounds while in mid-training, authors focus more on if there is complete rationale between audios and videos. 

<img width="1930" height="1087" alt="image" src="https://github.com/user-attachments/assets/10708824-6b70-48e6-900a-b0ba421a190a" />

