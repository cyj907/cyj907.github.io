---
layout: post
title: "[Read] Inception V3"
author: "Jade"
categories: read
date: 2018-04-17 10:17:00 --0800
---

A brief introduction:

> This is the version 3 of the inception model proposed by google
> to tackle image classification tasks.
> Inception modules were updated and several tips for building a deep network were proposed.
> I just have no idea why people doing fashion related work love this model so much.


# Problem formulation
Mainly solve the image classification task. But hopefully can transfer the model or the learned features for other tasks.

# Algorithm
The paper introduced several new inception modules to reduce the number of parameters and training time, but improve the quality of the captured features.

They list several __general design principles__:
1. avoid representational bottlenecks, which means that the dimensionality of the volume should be better increased every tile.
2. increase the activations per tile.
3. spatial aggregation.
4. balance the width and depth of the network.

> Actually, I don't quite understand the 3 and 4 points mentioned above.

The propose to factorize convolutions with large filter size:
## factorize into smaller convolutions
<img src="../assets/posts/2018-04-17/mini-net5x5.png">
> But what actually bothers me is that they don't use this structure in their codes.

## factorize into asymmetric convolutions
<img src="../assets/posts/2018-04-17/mini-net3x3.png">

## four different inception modules
1. the original one
<img src="../assets/posts/2018-04-17/ori-inception.png">

2. the modified one from 1
<img src="../assets/posts/2018-04-17/p3-inception.png">

3. another one using asymmetric convolution
<img src="../assets/posts/2018-04-17/p3-factor-inception.png">

4. the one suggested by principle 2
<img src="../assets/posts/2018-04-17/p2-inception.png">


> **Note**: the above modules should be used under different circumstances, as mentioned by the texts below images.

## Use auxiliary classifiers

They claimed that using an auxiliary classifier in higher level can be regarded as a regularizer to improve the classification results.

<img src="../assets/posts/2018-04-17/classifier.png">

## The inception v2 model
<img src="../assets/posts/2018-04-17/inception-v2.png">

## The inception v3 model
<img src="../assets/posts/2018-04-17/inception-v3.png">

The main difference between v2 and v3 is in the training procedure instead of network structure.
I did not mention label smoothing here. It is like adding some random noise to the labels to prevent overfitting.
