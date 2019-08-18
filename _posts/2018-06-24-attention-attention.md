yout: post
comments: true
title: "Attention? Attention!"
date: 2018-06-24 11:07:00
tags: attention rnn
image: "transformer.png"
---

> Attention has been a fairly popular concept and a useful tool in the deep learning community in recent years. In this post, we are gonna look into how attention was invented, and various attention mechanisms and models, such as transformer and SNAIL.

<!--more-->

<span style="color: #286ee0;">[Updated on 2018-10-28: Add [Pointer Network](#pointer-network) and the [link](https://github.com/lilianweng/transformer-tensorflow) to my implementation of Transformer.]</span><br/>
<span style="color: #286ee0;">[Updated on 2018-11-06: Add a [link](https://github.com/lilianweng/transformer-tensorflow) to the implementation of Transformer model.]</span><br/>
<span style="color: #286ee0;">[Updated on 2018-11-18: Add [Neural Turing Machines](#neural-turing-machines).]</span><br/>
<span style="color: #286ee0;">[Updated on 2019-07-18: Correct the mistake on using the term "self-attention" when introducing the [show-attention-tell](https://arxiv.org/abs/1502.03044) paper; moved it to [Self-Attention](#self-attention) section.]</span>

{: class="table-of-content"}
* TOC
{:toc}

Attention is, to some extent, motivated by how we pay visual attention to different regions of an image or correlate words in one sentence. Take the picture of a Shiba Inu in Fig. 1 as an example. 

![shiba]({{ '/assets/images/shiba-example-attention.png' | relative_url }})
{: style="width: 100%;" class="center"}
*Fig. 1. A Shiba Inu in a men’s outfit. The credit of the original photo goes to Instagram [@mensweardog](https://www.instagram.com/mensweardog/?hl=en).*

Human visual attention allows us to focus on a certain region with "high resolution" (i.e. look at the pointy ear in the yellow box) while perceiving the surrounding image in "low resolution" (i.e. now how about the snowy background and the outfit?), and then adjust the focal point or do the inference accordingly. Given a small patch of an image, pixels in the rest provide clues what should be displayed there. We expect to see a pointy ear in the yellow box because we have seen a dog’s nose, another pointy ear on the right, and Shiba's mystery eyes (stuff in the red boxes). However, the sweater and blanket at the bottom would not be as helpful as those doggy features.

Similarly, we can explain the relationship between words in one sentence or close context. When we see "eating", we expect to encounter a food word very soon. The color term describes the food, but probably not so much with "eating" directly.

