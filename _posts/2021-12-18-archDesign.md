---
layout: post
title: Digital Filters -- Under construction! 
subtitle: And a practical implementation of a Buttworth low pass filter
gh-repo: 
gh-badge: 
tags: 
katex: True
comments: true
---

A digital filter is a mathematical algorithm designed to operate on digital datsets, such as bio-signals recorded from sensors attached to a human body, to remove unwanted information or noise. Practical applications of digital filtering include removal of powerline noise and other unwanted components of a signal. In this way, it is simpler to analyze information of interest in the signal. It is important to pay attention to the type of filter and objectives when processing data. 

A class of digital filters classified as Linear time invariant systems (LTI) is commonly used in signal processing applications. In this article we will simply refer to them as digital filters. There are two main classes of digital filters:

* Finite impulse response (FIR)
* Infinite impulse response (IIR)

Their names derive from how the filters respond to a single pulse of inputThese filters are characterized by the length of their impulse responses, referred to as their impulse responses. 


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

