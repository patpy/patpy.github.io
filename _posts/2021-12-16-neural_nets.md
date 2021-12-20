---
layout: post
title: Neural Network
subtitle: Mathematical Description and Implementation in C++ 
gh-repo: 
gh-badge: 
tags: 
katex: True
comments: true
---

# **Background**
In this post we will provide a mathematical background and context of the backpropagation algorithm as well as discuss and implement, from ground up, a c++ library that can be used to create modular neuralnetworks.


| ![Crepe](/assets/img/perceptron.png){: .mx-auto.d-block :} |
|:--:|
| <b> Figure 1: The perceptron. A single neural processing unit. The inputs are multiplied with corresponding weights and linearly combined. The result is then fed to the activation functin to produce the output.</b> |

  
A reference to the image (\autoref{!Crepe}).

McCulloch and Pitts introduced the idea of biologically inspired computing machines(neural networks), and later Rosenblatt proposed the percetron as the first model of learning with examples (supervised learning)\textemdash \emph{McCulloch-Pitts} model of a neuron. 
The neural model consists of a linear combiner and a hard limiter as shown in **Figure 1**.

A single layer neural network as described above is limited to classification of linearly separable patterns . In practice, beacuse a one layer neural network is a limitation, we consider neural networks with than one layer\textemdash multilayer perceptrons.
Multilayer perceptrons are characterized by:
* Differentiable nonlinear activation function for each neuron
* one or more hidden layers (they are hidden from both input and output layers)
* High a degree of connectivity
  


In this post, we will restrict ourselves to feed-forward neural networks, as shown in see **Figure 2**. 

| ![nnet](/assets/img/neuralnet.png){: .mx-auto.d-block :} |
|:--:|
| <b> Figure 2: Neural network activation flows from one layer to another. A demonstration of a multilayer perceptron with D inputs and C output "neurons". .</b> |


The neural network arhitecture is “feed-forward” because nodes within a particular layer are connected only to nodes in the immediately “down-stream” layer. In this way, nodes in the input layer only activate nodes in the subsequent hidden layer. The subsequent hidden layer, in turn, will only activate nodes in the next hidden layer. This remains true until the nodes of the most down-stream hidden layer. The most down-stream hidden layer then feeds the output layer; see illustration in \cref{fig:multilayer-perceptron}. While every node is connected to every node in \cref{fig:multilayer-perceptron}, layers are not generally fully connected. Nodes from some layer, say *i* that innervate the *$$j^{th}$$* node in the subsequent layer *j* are in general a subset of the $$\textbf{I}$$ nodes that constitute the *$$i^{th}$$* layer. We denote this subset by $$\textbf{I}_k$$. So the weighted sum of the inputs is described as:

$$\begin{equation}
  \varphi_j = \sum_{i\in I_j}x_i,
\end{equation}
$$

where $$\textbf{I}_j$$ is the set of nodes from the *$$i^{th}$$* layer which feeds node *j*.

# **The backpropagation algorithm-Training Multilayer Perceptrons**

The backpropagation algorithm is the most popular method by which neural
networks are trained. We begin by specifying the parameters of our neural
network. As discussed in the previous section, feed-forward neural networks
on which we deploy our backpropagation learning algorithm consist of layers
classified as input, hidden, and output. In this configuration, there is only
one input layer and one output layer; the number of hidden layers is only
limited by available resources.
In general, a supervised learning algorithm attempts to minimize the
error between the actual outputs—activation at the output layer—and the
desired values. The algorithm achieves this objective by changing the values
of the weights in the network. Since the back–propagation is an iterative al-
gorithm, weights change incrementally. The change in weight is proportional
to its influence on the error: the bigger the influence of weight w i , the larger
the reduction of error induced by changing the weight. In other words, the
bigger the change our learning algorithm should make in that weight. Note
that this influence is not the same everywhere, and changing any particu-
lar weight generally makes all the other weights more or less influential on
the error. The process of adjusting weights is repeated until the error falls
below some desired threshold, at which point the algorthim is considered to
have learned the function of interest and the procedure terminates. For this
reason, back–propagation is also known as a steepest descent algorithm.

## **Derivation**
