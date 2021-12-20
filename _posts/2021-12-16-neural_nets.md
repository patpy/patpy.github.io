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

To describe the algorithm, let D = {x, d} be the set of example data. As
shown in Equation (3), y j is the output of neuron j in the output layer due
to input or simulus x. The error between desired value and neuron output
is exppressed as;

$$\begin{equation}
  e_j = d_j - y_j.
\end{equation}
$$

|![percep2](/assets/img/perceptron2.png){: .mx-auto.d-block :} |
|:--:|
| <b> Figure 3: Training the perceptron. Demonstration of backpropagation training with a single neural processing unit given a training dataset.</b> |

Figure 3 illustrates a neuron, j, being fed by a set of input neurons in the
previous layer (i th ). The induced local field φ j associated with neuron j is
described as,

$\begin{equation}
  \varphi_j = \sum_{i=0}^m w_{ji}y_ix_i,
\end{equation}
$$

where m is the number of neuronal inputs applied to neuron j. So y j , the
output of neuron j at some iteration n, is defined as;

$\begin{equation}
  y_j = f(\varphi_j),
\end{equation}
$$

where f (·) is the activation function.

We need to define a meaure of error for our back–propagation algorithm;
we consider the error energy each output node. It ensures that positive and
negative errors do not canel each other out. We modify Equation (2) by
square the differences before summing them. In addition, we scale the sum
of errors by 1/2 for convenience:

$$
\begin{equation}\label{eq:err}
  E \coloneqq \frac{1}{2} \sum_{i=1}^m(d_j - y_i)^2 = \frac{1}{2}\sum_{i=1}^me_j^2.
\end{equation}
$$
Note that \Cref{eq:err} assumes that the \emph{j}\(^{th}\) layer is the output  layer.

The change in a weight connecting a node in the previous layer to a node
in layer j is defined by,

$$
\begin{equation}\label{eq:weight_change}
  \Delta w_{ji} = -\alpha \frac{\partial E}{\partial w_{ji}}.
\end{equation}
$$

Note that $$\(\alpha\)$$ is a free parameter (learning rate) that we set at the beginning of training. With $$\(\alpha\)$$, we can set the step size that is appropriate for the problem at hand. The adjustment of $$\alpha\textemdash$$ between values of $$\(0\)$$ and \(1\)\textemdash is usually ad-hoc. The negative sign in the formular suggestes that the weights change in the direction of decreasing error. By the chain rule, we expand the partial derivative as follows:

$$
\begin{equation}\label{eq:chain_rule}
  \frac{\partial E}{\partial w_{ji}} = \frac{\partial E}{\partial e_j} \frac{\partial e_j}{\partial y_j} \frac{\partial y_j}{\partial \varphi_j} \frac{\partial \varphi_j}{\partial w_{ji}},
\end{equation}
$$

where $$\(\varphi_j\)$$ is the weigted sum of inputs into the $$\emph{j}\(^{th}\)$$ node. The partial derivative $$\(\partial E / \partial w_{ji}\)$$ represents a sensitivity factor for determining the direction of the serach in the weight space for synaptic weight $$\emph{w}\(_{ji}\)$$.

$$
\begin{equation}\label{eq:partial_local_field}
  \frac{\partial \varphi_j}{\partial w_{ji}} = y_i
\end{equation}
$$
