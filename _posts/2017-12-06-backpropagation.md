---
title: "Backpropagation"
date: 2017-12-06
permalink: /posts/2017/12/06/backpropagation
tags: 
  - neural network
  - backpropagation
---
<p align="center"><img src="/images/2017-12-06-backpropagation/cover.jpeg" alt="image"></p>

This would require a little bit of maths, so basic calculus is a pre-requisite.

In this post, I’ll try to explain backprop with a very simple neural network, shown below:

<!-- ![](/img/2017-12-06-backpropagation/01.jpeg) -->
<p align="center"><img src="/images/2017-12-06-backpropagation/01.jpeg" alt="image"></p>

L1, L2 and L3 represent the layers in the neural net. The numbers in square bracket represent the layer number. I’ve numbered every node on each layer. E.g. second node of first layer is numbered as 2[1], and so on.
I’ve labelled every weight too. E.g. the weight connecting second node of second layer (2[2]) to first node of third layer (1[3]) is w21[2].

I’ll assume that we’re using activation function g(z).

The basic concept behind backpropagation is to calculate error derivatives. After the forward pass through the net, we calculate the error, and then update the weights through gradient descent (using the error derivatives calculated by backprop).

If you understand [chain rule](http://www.mathcentre.ac.uk/resources/uploaded/mc-ty-chain-2009-1.pdf) in differentiation, you’ll easily understand backpropagation.

First, let us write the equations for the forward pass.

Let x1and x2 be the inputs at L1.

I’m denoting z as the weighted sum of previous layer, and a as the output of a node after applying non-linearity/activation function g.
> z1[2] = w11[1]*x1 + w21[1]*x2

> a1[2] = g(z1[2])

> z2[2] = w12[1]*x1 + w22[1]*x2

> a2[2] = g(z2[2])

> z3[2] = w13[1]*x1 + w23[1]*x2

> a3[2] = g(z3[2])

> z1[3] = w11[2]*a1[2] + w21[2]*a2[2] + w31[2]*a3[2]

> a1[3] = g(z1[3])

Now I’ll use MSE as a loss function.
> E = (1/2)×(a1[3]−t1)², where t1 is the target label.

To use gradient descent to backpropagate through the net, we’ll have to calculate the derivative of this error w.r.t. every weight, and then perform weight updates.

We need to find dE/dwij[k], where, wij is a weight at layer k.

By chain rule, we have

<!-- ![](/img/2017-12-06-backpropagation/02.jpeg) -->
<p align="center"><img src="/images/2017-12-06-backpropagation/02.jpeg" alt="image"></p>

Now, we can calculate these three terms on the RHS as follows,

<!-- ![](/img/2017-12-06-backpropagation/03.jpeg) -->
<p align="center"><img src="/images/2017-12-06-backpropagation/03.jpeg" alt="image"></p>

Therefore, we have

<!-- ![](/img/2017-12-06-backpropagation/04.jpeg) -->
<p align="center"><img src="/images/2017-12-06-backpropagation/04.jpeg" alt="image"></p>

Similarly,

<!-- ![](/img/2017-12-06-backpropagation/05.png) -->
<p align="center"><img src="/images/2017-12-06-backpropagation/05.png" alt="image"></p>

> Let δ1[3] = (a1[3] — t) * (g’(z1[3])).

Hence, we have

<!-- ![](/img/2017-12-06-backpropagation/06.png) -->
<p align="center"><img src="/images/2017-12-06-backpropagation/06.png" alt="image"></p>

Here, we’ll call δ1[3] as the error propagated by node 1 of layer 3.

Now, we want to go one layer back.

<!-- ![](/img/2017-12-06-backpropagation/07.jpeg) -->
<p align="center"><img src="/images/2017-12-06-backpropagation/07.jpeg" alt="image"></p>

On simplifying, we get

<!-- ![NOTE: The value of weight w11[2] is to be used before the update was performed on it. This applies to all equations and weights below.](/img/2017-12-06-backpropagation/08.jpeg) -->
<p align="center"><img src="/images/2017-12-06-backpropagation/08.jpeg" alt="image"></p>
> NOTE: The value of weight w11[2] is to be used before the update was performed on it. This applies to all equations and weights below.

Similarly,

<!-- ![](/img/2017-12-06-backpropagation/09.jpeg) -->
<p align="center"><img src="/images/2017-12-06-backpropagation/09.jpeg" alt="image"></p>

Similarly backpropagating through other nodes in L2, we get

<!-- ![](/img/2017-12-06-backpropagation/10.jpeg) -->
<p align="center"><img src="/images/2017-12-06-backpropagation/10.jpeg" alt="image"></p>

<!-- ![](/img/2017-12-06-backpropagation/11.jpeg) -->
<p align="center"><img src="/images/2017-12-06-backpropagation/11.jpeg" alt="image"></p>

In terms of δ, these can be written as

<!-- ![](/img/2017-12-06-backpropagation/12.png) -->
<p align="center"><img src="/images/2017-12-06-backpropagation/12.png" alt="image"></p>

When we have all the error derivatives, weights are updated as:

<!-- ![(wij[k] refers to weight wij at layer k)](/img/2017-12-06-backpropagation/13.png) -->
<p align="center"><img src="/images/2017-12-06-backpropagation/13.png" alt="image"></p>
<p align="center">
(wij[k] refers to weight wij at layer k)
</p>
where η is called the ‘learning rate’,

So, this was backprop for you! 

To get a better grasp of this, you can try deriving it yourself.

You can even do it for some other error functions like cross-entropy loss (with softmax). Or for a particular activation function like sigmoid, tanh, relu or leaky rely.
