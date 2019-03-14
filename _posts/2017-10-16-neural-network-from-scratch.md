---
title: "Neural Network From Scratch"
date: 2017-10-16
permalink: /posts/2017/10/16/neural-network-from-scratch
tags: 
  - neural network
---

<p align="center"><img src="/images/2017-10-16-neural-network-from-scratch/cover1.jpg" alt="image"></p>

Hello hackers, time for another coffee break!

This time, let’s start coding.

<!-- ![image](/images/2017-10-16-neural-network-from-scratch/01.gif) -->
<p align="center"><img src="/images/2017-10-16-neural-network-from-scratch/01.gif" alt="image"></p>

> The accompanying code can be found [here](https://github.com/sar-gupta/neural-network-from-scratch).

<script src="https://gist.github.com/sar-gupta/987315b240c01cfe0c028d42893c8d69.js"></script>

Numpy is used for mathematical calculations in Python.

Dill is used to store all variables in a python file, so that they can be loaded later. Install using pip3 install dill.

Now, we make a class for the neural_network:

<script src="https://gist.github.com/sar-gupta/3a3fb0abad6f9052a40ec0822b9e9677.js"></script>

And a class for layer:

<script src="https://gist.github.com/sar-gupta/9af0d730c4ec0ed7d6712766c8f15569.js"></script>

Okay, so let’s dive in!

Class neural_network has the following functions:

* __init__ : It takes 4 things as inputs:
1. num_layers: number of layers in the network.
2. num_nodes: It is a list of size num_layers, specifying the number of nodes in each layer.
3. activation_function: It is also a list, specifying, the activation function for each layer (activation function for first layer will usually be None. It can take values sigmoid, tanh, relu, softmax.
4. cost_function: Function to calculate error between predicted output and actual label/target. It can take values mean_squared, cross_entropy.
 — Layers are initialized with the given number of nodes in each layer. Weights associated with every layer are initialized.

* train : It takes 6 inputs:
1. batch_size: Mini batch size for gradient descent.
2. inputs: Inputs to be given to the network.
3. labels: Target values.
4. num_epochs: Number of epochs i.e. how many times the program should iterate over all training .
5. learning_rate
6. filename: The name of the file that will finally store all variables after training. (filename must have the extension .pkl).
 — First, there is a loop for iterating over number of epochs. Then, there is a nested loop for iterating over all mini batches. After that, forward_pass, calculate_error and back_pass are called over the mini batch.

* forward_pass : It takes just 1 input:
1. inputs: Mini batch of inputs.
 — This function multiplies inputs with weights, applies the activation function and stores the output as activations of next layer. This process is repeated for all layers, until we have some activations in the output layer.

* calculate_error : It also takes 1 input:
1. labels: Mini batch of labels.
 — This function calculates the error between the predicted output (i.e. activations at the output layer after a forward pass) and the target values. This error is then backpropagated through the network in back_pass function.

* back_pass : It takes 1 input:
1. labels: Mini batch of labels.
 — This function implements the backpropagation algorithm. This algorithm will be discussed in detail in a separate post. Basically what it does is, it calculates the gradient, multiplies it with a learning rate and subtracts the product from the existing weights. This is done for all layers, from the last to the first.

* predict : It takes 2 inputs:
1. filename: The file from which trained model is to be loaded.
2. input: The input for which we want the prediction.
 — It does a forward pass, and then converts the output into one-hot encoding i.e. the maximum element of array is 1 and all others are 0.

* check_accuracy : It takes 3 inputs:
1. filename: The file from which trained model is to be loaded.
2. inputs: Input test data.
3. labels: Target test data.
 — This function does pretty much the same thing as predict. But instead of returning the predicted output, it commpares the predictions with the labels, and then calculates accuracy as correct*100/total.

Class layer has the following functions:

* __init__ : It takes 3 arguments:
1. num_nodes_in_layer: Number of nodes in that layer.
2. num_nodes_in_next_layer: Number of nodes in the next layer.
3. activation_function: Activation function for that layer.
 — This function is called from the constructor of neural_network class. It initializes one layer at a time. The weights of the last layer are set to None.

This post explained the code in detail. Although convolutional neural networks (CNNs) perform much better on images, I trained a neural network on MNIST just for the feel of it. CNNs will be covered in a later blog post.

<!-- ![image](/images/2017-10-16-neural-network-from-scratch/02.jpeg) -->
<p align="center"><img src="/images/2017-10-16-neural-network-from-scratch/02.jpeg" alt="image"></p>

I hope you can now implement a neural network from scratch yourself. Happy coding!