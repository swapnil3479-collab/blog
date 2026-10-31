---
title: "Convolutional Neural Networks Explained: How AI Learns to Recognize Images"
date: 2026-10-31
slug: "convolutional-neural-networks-explained-how-ai-learns-to-recognize-ima"
description: "In the last post, we saw that a computer sees a photo as a grid of numbers, and that a small grid called a kernel can slide across it to find edges. The catch: "
draft: false
tags:
  - "Computer Vision"
  - "Deep Learning"
cover:
  image: "/blog/images/convolutional-neural-networks-explained-how-ai-learns-to-recognize-ima-cover.svg"
  alt: "Convolutional Neural Networks Explained: How AI Learns to Recognize Images"
  relative: false
  hidden: false
---

In the last post, we saw that a computer sees a photo as a grid of numbers, and that a small grid called a kernel can slide across it to find edges. The catch: a human had to write those numbers. Someone sat down and decided that [-1, 0, 1] finds edges.

Convolutional Neural Networks (CNNs) flip that around. You don't write the filters. You show the network thousands of labeled images, and it *discovers* the filters that work. That one idea is why your phone can sort photos by "dog" and "beach."


![Key Statistics](/blog/images/convolutional-neural-networks-explained-how-ai-learns-to-recognize-ima-infographic.svg)


## Why a regular neural network chokes on images

Before CNNs, the obvious approach was a standard fully connected network: one neuron per pixel, every neuron connected to every neuron in the next layer. It falls apart almost immediately.

- A tiny 32 × 32 color image (the CIFAR-10 size) has 3,072 values, so each neuron in the first layer needs 3,072 weights. Manageable.
- A 200 × 200 color image means **120,000 weights per neuron**.
- A 224 × 224 color image has **150,528 input values**. Add just three modest hidden layers of 128 neurons and you pass **300 billion trainable parameters**.

That's too slow to train, and so many parameters mean the model memorizes its training images instead of learning anything general (overfitting).

There's a subtler problem too. A fully connected network is not *translation invariant*: shift the cat three pixels to the left and, as far as the network is concerned, it's a brand new input.

## The three ideas that fix it

CNNs make an explicit assumption: **the input is an image.** That assumption buys three powerful tricks.

1. **Local receptive fields.** Each neuron looks at only a small patch of the image, not the whole thing. Nearby pixels are what form edges and textures, so this is where the information is.
2. **Weight sharing.** The *same* filter slides across the whole image. A filter that detects a vertical edge in the top-left corner works equally well in the bottom-right. This slashes the parameter count and gives the network translation invariance for free.
3. **Pooling.** After filtering, shrink the result by keeping only the strongest responses in each small region. Detail drops, but the important signal survives.

## What's inside a CNN, layer by layer

Take a 32 × 32 × 3 image going through a typical CNN:

- **Convolutional layer.** A set of learnable filters (often 3 × 3 or 5 × 5) slide over the image, each producing a *feature map* showing where that pattern appears. Use 12 filters and the output is 32 × 32 × 12.
- **Activation (ReLU).** A simple non-linear function applied to every value. Without it, stacking layers would collapse into one big linear operation and the network couldn't learn anything complex.
- **Pooling layer.** 2 × 2 max pooling with a stride of 2 halves width and height: 32 × 32 × 12 becomes 16 × 16 × 12. Cheaper to compute, and more tolerant of small shifts.
- **Flatten.** Unroll the final maps into one long vector (16 × 16 × 12 gives 3,072 numbers).
- **Fully connected layer.** Combine all the detected features to reason about the whole image.
- **Output (Softmax).** Convert final scores into probabilities. With 10 classes, you get 10 numbers that sum to 1.

Many networks also use **dropout**, which randomly switches off a fraction of neurons during training so the network can't lean too hard on any single one.

## The big payoff: a hierarchy of features

Here's the part that feels almost magical. Nobody tells the early layers to look for edges. Trained on enough images, they *learn* to, because edges are useful. Then:

- **Early layers** respond to edges and colors.
- **Middle layers** combine those into textures and shapes.
- **Deep layers** combine *those* into object parts, and eventually whole objects.

It's the same progression you saw in a hand-built filter, except learned automatically and stacked dozens of layers deep. The CNN's structure is often described as a *backbone* (the feature extractor) followed by a *head* (the classifier).

## From LeNet to the deep learning era

The idea is older than most people think. CNNs trace back to Yann LeCun's work in the late 1980s. **LeNet-5**, introduced in 1998, became the best-known early architecture and was used for handwritten digit recognition. It was tiny by today's standards: 2 convolutional layers and 3 fully connected layers, about **60,000 parameters**.

It worked, but it was limited by the computing power and data of its time. The idea then waited for hardware and datasets to catch up. When they did, deeper architectures followed: AlexNet, VGG (the 16-layer VGG-16 is a classic teaching example), GoogLeNet, and more. Each one stacked more layers and added tricks, and each pushed accuracy on image benchmarks higher.

## Where CNNs still struggle

CNNs aren't perfect. They only know what their training data showed them. Dark lighting, unusual angles, or low resolution can make a confident model completely wrong. They also tend to be better at local patterns than at relationships between distant parts of an image, a limitation that has pushed researchers toward a newer architecture.

That's the subject of the next post: **Vision Transformers**, and the question of whether attention can replace convolution.

## References

1. [Convolutional Neural Network (CNN) Architectures (GeeksforGeeks)](https://www.geeksforgeeks.org/machine-learning/convolutional-neural-network-cnn-architectures/)
2. [CS231n: Convolutional Neural Networks for Visual Recognition](https://cs231n.github.io/convolutional-networks/)
3. [Introduction to Convolution Neural Network (GeeksforGeeks)](https://www.geeksforgeeks.org/machine-learning/introduction-convolution-neural-network/)
4. [How do Convolutional Neural Networks (CNNs) work? (GeeksforGeeks)](https://www.geeksforgeeks.org/computer-vision/how-do-convolutional-neural-networks-cnns-work/)
5. [Convolutional Neural Network: A Complete Guide (LearnOpenCV)](https://learnopencv.com/understanding-convolutional-neural-networks-cnn/)