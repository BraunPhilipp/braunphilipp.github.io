---
title:  "Transformers"
mathjax: true
layout: post
categories: machine-learning
---

In recent years, transformers have gained considerable popularity through networks like XLNet, BERT and GPT-3. The backbone of these networks is usually built using an encoder-decoder structure. The encoder maps the input embedding to a higher dimensional space that is then used by the decoder to derive output probabilities. This structure was previously used in RNNs with LSTM or GRU cells, but not in feed-forward networks. The reason for this was a missing mechanism for modelling relationships within Seq2Seq models through a feed-forward pass. To combat this issue Vaswani et al. [1] introduced the concept of multi-head attention and positional encodings through sine and cosine functions. The structure of their network is shown in Figure 1. The left part of the figure refers to the encoder, while the right part forms the decoder. The decoder stage is initialised using an empty token at the beginning of each sequence. 


![Transformer](/assets/img/transformer.png)
<small><center>Figure 1: Transformer (Model Architecture)</center></small>

## Attention

The multi-headed attention blocks in Figure 1 are built using multiple blocks of scaled dot-product attention. Scaled-dot product attention maps a set of key-value pairs to an output via the formula provided in Figure 2. Q contains the query (vector representation of one word in the sequence), K are all the keys (vector representations of all the words in the sequence) and V are all the values, which are again the vector representations of all the words in the sequence. Q, K and V are derived through a matrix multiplication with learnable weight matrices. A simplified example is provided in Figure 3. Note that Q and V only refer to the same word sequence at the beginning of the encoder and decoder stage. In later stages the output of encoder stages is used within the decoder. Note that d_k is used to avoid saturation of the softmax function.

![Attention](/assets/img/scaled-attention-1.png)
<small><center>Figure 2: Scaled Dot-Product Attention (Formula)</center></small>

![Attention](/assets/img/1_1je5TwhVAwwnIeDFvww3ew-1.gif)
<small><center>Figure 3: Attention (Simplified Example) [3]</center></small>

## Positional Encoding

Since transformers do not contain any recurrence in the network, the relative or absolut position of each word needs to be injected into the sequence. The original formula for deriving positional encodings is provided in Figure 4. pos refers to the position and i is the dimension of the word within the sequence. Therefore, each position and dimension refer to a separate frequency in the positional encoding.

![Position](/assets/img/position.png)
<small><center>Figure 4: Positional Encoding (Formula)</center></small>

## Implementation

Since transformers perceive the entire sequence at once and do not need recurrent connections, they can be easily accelerated using modern hardware accelerators. The Hugging Face repository [5] provides many pretrained SOTA transformers for various NLP tasks.


## References

1. [https://arxiv.org/pdf/1706.03762.pdf](https://arxiv.org/pdf/1706.03762.pdf)
2. [https://jalammar.github.io/illustrated-transformer/](https://jalammar.github.io/illustrated-transformer/)
3. [https://towardsdatascience.com/illustrated-self-attention-2d627e33b20a](https://towardsdatascience.com/illustrated-self-attention-2d627e33b20a)
4. [https://medium.com/inside-machine-learning/what-is-a-transformer-d07dd1fbec04](https://medium.com/inside-machine-learning/what-is-a-transformer-d07dd1fbec04)
5. [https://github.com/huggingface/transformers](https://github.com/huggingface/transformers)
