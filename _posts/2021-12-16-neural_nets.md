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

**Background**
In this post we will provide a mathematical background and context of the backpropagation algorithm as well as discuss and implement, from ground up, a c++ library that can be used to create modular neuralnetworks.

[image]://assets/img/perceptron.png
*The perceptron is the most wonderful thing ever discovered*

A reference to the [image](#image)

![Crepe](//assets/img/perceptron.png){: .mx-auto.d-block :}

\begin{figure}[htbp]
\centering
\includegraphics[keepaspectratio, width=\textwidth,  height=0.75\textheight]{/_images/perceptron.png}
\caption{Test}
\label{perceptron}
\end{figure}

\\( a^2 = b^2 \\)

A reference to the image (\autoref{perceptron}).

McCulloch and Pitts introduced the idea of biologically inspired computing machines(neural networks), and later Rosenblatt proposed the percetron as the first model of learning with examples (supervised learning)\textemdash \emph{McCulloch-Pitts} model of a neuron. 
The neural model consists of a linear combiner and a hard limiter as shown in \cref{fig:perceptron}.

A single layer neural network as described above is limited to classification of linearly separable patterns . In practice, beacuse a one layer neural network is a limitation, we consider neural networks with than one layer\textemdash multilayer perceptrons.
Multilayer perceptrons are characterized by:
* Differentiable nonlinear activation function for each neuron
* one or more hidden layers (they are hidden from both input and output layers)
* High a degree of connectivity
  


In this post, we will restrict ourselves to feed-forward neural networks. The neural network arhitecture is “feed-forward” because nodes within a particular layer are connected only to nodes in the immediately “down-stream” layer. In this way, nodes in the input layer only activate nodes in the subsequent hidden layer. The subsequent hidden layer, in turn, will only activate nodes in the next hidden layer. This remains true until the nodes of the most down-stream hidden layer. The most down-stream hidden layer then feeds the output layer; see illustration in \cref{fig:multilayer-perceptron}. While every node is connected to every node in \cref{fig:multilayer-perceptron}, layers are not generally fully connected. Nodes from some layer, say *i* that innervate the *$$j^{th}$$* node in the subsequent layer *j* are in general a subset of the $$\textbf{I}$$ nodes that constitute the *$$i^{th}$$* layer. We denote this subset by $$\textbf{I}_k$$. So the weighted sum of the inputs is described as:

$$\begin{equation}
  \varphi_j = \sum_{i\in I_j}x_i,
\end{equation}
$$

where $$\textbf{I}_j$$ is the set of nodes from the *$$i^{th}$$* layer which feeds node *j*.

**FIR filters**
In FIR filter configuration, the output is not fed-back to the input and so if there is no subsequent input there will be no output. The output of an FIR filter is given by
 
 $$y(n) = \sum_{k=0}^{p-1} b_kx(n-k)$$
 where x(n) is the original signal and $$b_k$$ are the filter coefficients. The all-zero structure of FIR filters guarantees that they are always stable. FIR filters are appropriate for applications where linear phase is important.  One practical consideration of FIR filters is that they require a lot of computing resources (memory since a high number of coefficeints is needed to achieve comparable performance to IIR filters) when compared to IIR filters. The simplest FIR filter is the rectangular windlow (normalized) which does a moving a average of the signal. It acts as a low pass filter (smoothes the signal).  

Pros of FIR filters

*    Linear phase: Fit is possible to design linear phase FIR filters. The benefit here is that there will be no phase distortion introduced into the filtered signal because all frequencies are shifted in time by the same amount (constang group and phase delay).    
*    Stability: Since FIR filters never use any previous output values, when computing present output (they have no feedback), it is impossible for them to become unstable for any type of input signal. 
*    Arbitrary frequency response: Optimizations algorithms such as Parks-McClellan enable for the design of an FIR with an arbitrary magnitude response. This means that an FIR can be customised more easily when compared to IIR filters.
*    Fixed point performance: FIR filters are more robust to effects of quantisation IIR filters.

Cons

*    High computational and memory burden: FIR filters generally require many more coefficients to achieve a good cut-off when compared to their IIR counterparts. 
*    Higher latency: in applications where fast high throughput is critical, FIR filters do not perform satisfactorily because of the higher number of coefficients. This is especially challenging in real-time closed-loop control applications, where an FIR filter may have too much group delay to achieve loop stability.

**IIR filters**
In IIR filter configurations, the output is fed-back to its input. The response to a single impulse is therefore a gradual decay that will never technically reach zero even though it may quickly drop towards zero (no output). IIR filters are suitable in applications that do not require linear phase, and when memory is limited. 
The output of an IIR filter is given by 

$$ y(n) = \sum_{k=0}^{p_a - 1} a_ky(n-k) + \sum_{k=0}^{p_b - 1}b_kx(n-k)$$

where the first term includes the feedback coefficients and the second term is the FIR output model. This type of filter is also known as an Autoregressive Moving Average (ARMA) model (the first term being the Autoregressive part). 


Advantages

*    Low implementation cost: less coefficients and memory are needed to achieve similar performance of FIR filters (cut-off frequency and stopband attenuation).
*    Low latency: since fewer coefficients are needed to achieve good performance, IIR filters are suitable for real-time control and very high-speed RF applications.
*    Analog equivalent: IIR filters are digital analogs of anlog filters.

Disadvantages

*    Non-linear phase characteristics: IIR filters are characterized with nonlinear behavior especially near the cut-off frequencies. It is possible to improve on passband phase properties by using all-pass equalisation filters.
*    More detailed analysis: IIR filters requires more scaling and numeric overflow analysis when implemented in fixed point, compared to FIR filters. The Direct form II filter structure is especially sensitive to the effects of quantisation, and requires special care during the design phase.
*    Numerical stability: Less numerically stable than their FIR counterparts, due to the feedback.


IIR filters can be designed by converting analog filters into the above IIR digital form. Examples include Butterworth, Chebyshev, Elliptic and Bessel filters.



This is a demo post to show you how to write blog posts with markdown.  I strongly encourage you to [take 5 minutes to learn how to write in markdown](https://markdowntutorial.com/) - it'll teach you how to transform regular text into bold/italics/headings/tables/etc.

**Here is some bold text**

## Here is a secondary heading

Here's a useless table:

| Number | Next number | Previous number |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |


How about a yummy crepe?

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg)

It can also be centered!

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .mx-auto.d-block :}

Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.
