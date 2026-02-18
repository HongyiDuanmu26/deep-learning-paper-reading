# [QWEN TECHNICAL REPORT](https://arxiv.org/pdf/2309.16609)

vague description of data preparation:
* For public web data, we extract text from HTML and use language identification tools to determine the language.
* To increase the diversity of our data, we employ deduplication techniques, including exact-match deduplication after normalization and fuzzy deduplication using MinHash
  and LSH algorithms.
* To filter out low-quality data, we employ a combination of rule-based and machine-learning-based methods. Specifically, we use multiple models to score the content, including
  language models, text-quality scoring models, and models for identifying potentially offensive or inappropriate content.
* To further enhance the quality of our data, we selectively up-sample data from certain sources, to ensure that our models are trained on a diverse range of high-quality content.
* byte pair encoding (BPE) as our tokenization method. To enhance the performance of our model on multilingual downstream tasks, particularly in Chinese,
  we augment the vocabulary with commonly used Chinese characters and words, as well as those in other languages. We have split numbers into single digits.
* Finally, we have built a dataset of up to 3 trillion tokens. The final vocabulary size is approximately 152K.

Architecture:
* we have opted for the untied embedding approach instead of tying the weights of the input embedding and output projection. Larger model.
* RoPE as positional embedding and use FP32 precision for the inverse frequency matrix, rather than BF16 or FP16.
* For most layers, we remove biases except QKV layer of attention.
* RMSNorm and pre-norm in transformers.
* SwiGLU as activation function.
* Reduced the dimension of the feed-forward network (FFN) from 4 times the hidden size to 8/3 of the hidden size.

Most architectural modifications are not novel.
