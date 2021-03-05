---
title:  "Vanishing Gradients"
mathjax: true
layout: post
categories: machine-learning
---

Vanishing gradients are one of the primary issues within deep neural networks. During backpropagation gradients are calculated via the chain rule by multiplying local gradients within each layer. Therefore, gradients within neural networks become increasingly smaller with the number of layers within the network. The problem is usually amplified through the activation function choice. For instance, choosing a sigmoid activation function will usually underperform ReLU or Leaky ReLU, since gradients are overall smaller in sigmoid activated networks. Figure 1 shows the training accuracy of a network with different activation functions. Sigmoid (blue), ReLU (red) and Leaky ReLU (green) performance are shown for 2400 training epochs.


![Vanish](/assets/img/vanish.png)
<small><center>Figure 1: Training Accuracy [1]</center></small>

Note that ReLU activated networks can also encounter vanishing gradients based on weight sign changes. To further improve training accuracy other activation functions, network structures and data representations can be explored. In particular, ResNets have gained considerable popularity by feeding the input identity into subsequent layers as shown in Figure 2. Other data representations may also lead to different loss functions that outperform based on dataset distributions [4]. In terms of regularization, L2 regularization will decrease weights and may lead to vanishing gradients. This can be avoided by implementing elastic net regularization [5].

![Vanish](/assets/img/resblock.png)
<small><center>Figure 2: Residual Block [2]</center></small>


## References

1. [https://www.mygreatlearning.com/blog/the-vanishing-gradient-problem/](https://www.mygreatlearning.com/blog/the-vanishing-gradient-problem/)
2. [https://arxiv.org/pdf/1512.03385.pdf](https://arxiv.org/pdf/1512.03385.pdf)
3. [https://github.com/pytorch/vision/blob/master/torchvision/models/resnet.py](https://github.com/pytorch/vision/blob/master/torchvision/models/resnet.py)
4. [https://stackoverflow.com/questions/49322197/comparing-mse-loss-and-cross-entropy-loss-in-terms-of-convergence](https://stackoverflow.com/questions/49322197/comparing-mse-loss-and-cross-entropy-loss-in-terms-of-convergence)
5. [https://en.wikipedia.org/wiki/Elastic_net_regularization](https://en.wikipedia.org/wiki/Elastic_net_regularization)