# [Enhancing Multi-Image Understanding through Delimiter Token Scaling](https://arxiv.org/abs/2602.01984)

There is a special delimiter token between images in VLM models' inputs, \<vision satart\> and \<vision end\>. It serves like: 1. aggregation of the corresponding image feature block. i-th delimiter attends to only i-images mainly. 2. a representative of i-th image in interimage information fusion.
The finding close to [The Narrow Gate: Localized Image-Text Communication in Vision-Language Models](https://arxiv.org/abs/2412.06646v2) [[Note](https://github.com/HongyiDuanmu26/deep-learning-paper-reading/blob/main/Notes/Narrowgate.md)].
<img width="2023" height="972" alt="image" src="https://github.com/user-attachments/assets/bfb4da3c-5444-4dc8-99c2-e33ad5ca8f9b" />

And the authors think this mechanism is not emphasized enough. After trying some methods, they finally find that scaling up the hidden state of the delimiter before the transformer layers is the best way. 
Simply, if it is a delimiter, multiply by a scale larger than 1. It will make QK results larger and attention larger after softmax.
<img width="693" height="214" alt="image" src="https://github.com/user-attachments/assets/6854b83c-c6d9-4b18-b954-4f034613c392" />

The attention map is clearer after scaling, which means attention across images is less and delimeters are more aggregated and more representative of the corresponding image.

<img width="2008" height="934" alt="image" src="https://github.com/user-attachments/assets/cc240571-609c-478c-9035-f77d24a7046d" />
