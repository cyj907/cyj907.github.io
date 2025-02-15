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

<img src="../assets/posts/2018-04-16/bilstm.png">
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

Real outfits and fake outfits consisting of randomly chosen items are selected half-half as the test set. All models are required to distinguish between good and bad outfits. The evaluation criteria are prediction accuracy and AUC.
> I don't think this test is fair for __SetRNN__ either. Because negative outfits selected might not be counted as an outfit. The training data pair for __SetRNN__ are popular outfits and unpopular outfits. So, the model might learn to tell good outfits from bad ones, but not outfits from non-outfits.

## Results
Below are the results for the above two tests.
<img src="../assets/posts/2018-04-16/table.png">

We can see that __Bi-LSTM__ greatly outperforms __SiameseNet__ and __SetRNN__ in __fill-in-the-blank__ test. For __compatibility test__, both __SiameseNet__ and __Bi-LSTM__ greatly outperforms __SetRNN__. __Bi-LSTM__ outperforms __SiameseNet__ by about 4% (85% vs 89%). Here, I want to explain the results from my perspective.

> First, let's talk about the __fill-in-the-blank__ test. The __Bi-LSTM__ model uses extra information as __blank position__ for prediction. And we know that as the seed items are arranged in order, the __blank position__ provides information about the category of the items. But for __SiameseNet__ and __SetRNN__, they do not have any knowledge about the position ahead of time. This might be the reason why __Bi-LSTM__ significantly outperforms the other two.
> Second, let's talk about the __compatibility test__. If you watch the table carefully, you might see that the visual semantic embedding does not play an important role in improving the results, but the structure of RNN. For models using a simple RNN, the performance are not so good. But for models using LSTM or GRU as basic blocks, the performance improves a lot. Therefore, I might say that LSTM and GRU is actually helping the results __A LOT__. But the overall idea that treats an outfit as a sequence might not be the core for improvements.
