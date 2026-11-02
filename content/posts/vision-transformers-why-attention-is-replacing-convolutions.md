---
title: "Vision Transformers: Why Attention Is Replacing Convolutions"
date: 2026-11-02
slug: "vision-transformers-why-attention-is-replacing-convolutions"
description: "For about a decade, convolutional neural networks were the undisputed kings of computer vision. If a machine needed to look at a picture, a CNN did the looking."
draft: false
tags:
  - "Computer Vision"
  - "Transformers"
cover:
  image: "/blog/images/vision-transformers-why-attention-is-replacing-convolutions-cover.svg"
  alt: "Vision Transformers: Why Attention Is Replacing Convolutions"
  relative: false
  hidden: false
---

For about a decade, convolutional neural networks were the undisputed kings of computer vision. If a machine needed to look at a picture, a CNN did the looking. Then a paper with a charming title, *"An Image is Worth 16x16 Words,"* argued that the same architecture behind ChatGPT-style language models could do the job too, with no convolutions at all.

That architecture is the **Vision Transformer (ViT)**, and it has reshaped how modern vision systems are built. This post explains how it works, and what it gives up and gains compared with the CNNs we covered last time.


![Key Statistics](/blog/images/vision-transformers-why-attention-is-replacing-convolutions-infographic.svg)


## The one-sentence idea

Treat an image like a sentence. Cut it into patches, call each patch a "word," and let a transformer figure out how the words relate to each other.

## How a ViT works, step by step

**1. Cut the image into patches.** A 224 × 224 image split into 16 × 16 pixel patches gives you **196 patches**. Each patch is flattened into a single vector (a P × P × C patch becomes a vector of length P² × C), much like a word becoming a token.

**2. Embed each patch.** A learned linear projection maps every flattened patch to a vector of fixed size, exactly like word embeddings in language models.

**3. Add position information.** Transformers treat their inputs as an unordered set, so the model has no idea which patch came from the top-left and which from the bottom-right. Positional encodings are added to each patch embedding to put the spatial order back.

**4. Prepend a CLS token.** A special learnable "classification" token is added to the front of the sequence. As the data flows through the network, it gathers information from every patch and becomes the summary used for the final prediction, an idea borrowed from BERT in language modeling.

**5. Run the transformer encoder.** The sequence passes through stacked layers of **self-attention** and feed-forward networks. Self-attention is the star: it computes how much each patch should "pay attention" to every other patch. A patch containing a dog's ear can directly consult a patch containing its tail on the other side of the image.

**6. Classify.** The final CLS token output goes to a small classification head, which produces the prediction.

## What attention gives you that convolution doesn't

A CNN builds understanding from the bottom up. Each filter sees only a small neighborhood, and the network's *receptive field* only grows as you stack layers. Far-apart parts of an image can only talk to each other deep in the network.

A ViT has a **global view from the very first layer**. Every patch can attend to every other patch immediately. That makes it good at capturing long-range relationships, which matters for understanding whole scenes rather than just local textures.

Transformers are also highly parallelizable and scale well to enormous models, which is a big part of why they took over language AI.

## The catch: no built-in assumptions

Remember the CNN tricks from last time: local receptive fields, weight sharing, pooling. Those encode a belief that *nearby pixels matter most and a feature is the same anywhere in the image.* This is called an **inductive bias**, and it's like a jigsaw solver assuming pieces connect by shape and color. It speeds learning.

ViTs have far less of this built in. The result:

| | CNN | Vision Transformer |
|---|---|---|
| Core operation | Convolution + pooling | Self-attention |
| Receptive field | Local, grows with depth | Global from the start |
| Inductive bias | Strong | Minimal |
| Data needs | Works well with smaller datasets | Wants large-scale data |
| Scaling | Harder to scale up | Scales very well |

That data hunger is real. The original ViT reached its best results when pretrained on Google's huge proprietary JFT-300M dataset. Pretrained on the smaller, open ImageNet-21k, it only matched the best convolutional ResNet models rather than beating them.

## Why naive attention was a problem

Self-attention gets expensive fast: its cost grows with the **square** of the sequence length. A sentence of 1,000 words means 1,000 tokens, which is fine. But treat every *pixel* of a modest 300 × 300 × 3 image as a token and you'd have around 270,000 tokens and an attention map with roughly **72.9 billion** entries.

The ViT's key trick was to sidestep this by using **patches** instead of pixels. Going from 270,000 tokens to a couple hundred makes global attention affordable.

## The family tree

The original ViT inspired a wave of improvements aimed at its weak spots:

- **Data-efficient variants** like DeiT, which make ViTs trainable without giant private datasets.
- **Hierarchical designs** like Swin, which bring back CNN-like multi-scale structure for tasks like detection and segmentation.
- **Tokens-to-Token ViT**, which changes how images are tokenized to cut the need for massive pretraining.
- **Self-supervised and lightweight versions**, which narrow the gap with CNNs on small datasets and cheaper hardware.

ViTs now show up in image classification, object detection, segmentation, medical imaging, autonomous driving, and generative modeling.

## So is attention really replacing convolution?

Mostly it's *merging* with it. The honest picture in 2026: ViTs dominate when you have lots of data and compute, CNNs remain efficient and strong when resources are tight, and many of the best modern models blend ideas from both. A bonus for the bigger picture: because ViTs speak the same language as text transformers, they are a natural fit for the multimodal models that read images and text together, a topic we'll get to later in this series.

Next up: **object detection**, where the question changes from "what's in this image?" to "what is it, and exactly where?"

## References

1. [Vision Transformers (ViT) in Image Recognition (GeeksforGeeks)](https://www.geeksforgeeks.org/computer-vision/vision-transformers-vit-in-image-recognition/)
2. [Vision Transformer (ViT) Architecture (GeeksforGeeks)](https://www.geeksforgeeks.org/deep-learning/vision-transformer-vit-architecture/)
3. [Vision Transformers Explained: The Future of Computer Vision? (Roboflow)](https://roboflow.com/blog/vision-transformers)
4. [Vision Transformers (ViT) Tutorial: Architecture and Code Examples (DataCamp)](https://www.datacamp.com/tutorial/vision-transformers)
5. [Vision Transformers (ViT) Explained: Are They Better Than CNNs? (Towards Data Science)](https://towardsdatascience.com/vision-transformers-vit-explained-are-they-better-than-cnns/)
6. [Vision Transformers, Explained (Towards Data Science)](https://towardsdatascience.com/vision-transformers-explained-a9d07147e4c8/)