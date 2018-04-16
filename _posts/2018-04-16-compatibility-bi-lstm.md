---
layout: post
title: "[Read] Fashion Compatibility with Bi-LSTMs"
author: "Jade"
categories: read
date: 2018-04-16 09:15:00 --0800
---

A brief introduction:

> The work attempts to solve the following three tasks:
> - suggests an item that matches the existing items in an outfit
> - recommend the whole outfit with image or text specifications by users
> - predict the outfit compatibility

# Problem formulation
See the introduction above

# Algorithm
The proposed work assumes an outfit as a sequence of items and models the cooccurrence of different items in an outfit using a Bi-LSTM.
They also adopts multi-modal data, including images and texts to build the model. The outfit item sequences are of top-to-bottom-to-accessory order.
Two key points lie in their algorithm:
- Bi-LSTM to model the likelihood of the sequences
- a so-called visual semantic embedding to match the images to the corresponding description texts. They minimize the cosine distances between images and the paired texts,
but maximize the distances between images and the unpaired texts.

**Note**: the image embedding are extracted from Inception V3 model. And the parameters of the model was fine tuned.

# Comparison study
## Methods to be compared
- SiameseNet

They project the image representation from 2048D to 512D and use cosine distance with margin as 0.8.
The compatibility of an outfit is obtained by mean pair-wise distance.

- SetRNN

The work mentioned by previous post. Actually I don't think the comparison is fair because as mentioned in the previous post,
the pooling method that achieves best performance is average pooling. But in this paper, they use RNN pooling.

## Tests for evaluation
- Fill-in-the-blank recommendation

Outfits in the test set were selected as the questions. One item for each outfit is taken out as one of the answer choice, and the remaining items become seed items.
Three other randomly chosen items were added as confusion choice. The evaluation criteria are the prediction accuracy.
> This test is similar to the one proposed in __SetRNN__. But I think it is not as reasonable as the previous one. Because the randomly picked items can be a match
for the seed items, although they never appeared in the same outfit in the dataset. But indeed, it is not economic to always hire some fashion experts to curate the test dataset, so the test used in this very work is acceptable.

- Compatibility test
