---
layout: post
title: The Writing's on the Wall, Part 1 (Intro to Keras)
categories: Neural-Network Keras Kaggle
published: False
---

*Visions of the future are never completely true, but with the right key, some can be more truthful than others.  Here's an introduction to the neural network library, Keras, which I used when predicting handwritten numbers.*

<!--more-->

>*"Oneiroi are beyond our unravelling --who can be sure what tale they tell? Not all that men look for comes to pass. Two gates there are that give passage to fleeting Oneiroi; one is made of horn, one of ivory. The Oneiroi that pass through sawn ivory are deceitful, bearing a message that will not be fulfilled; those that come out through polished horn have truth behind them, to be accomplished for men who see them." Homer, Odyssey 19. 562 ff (Shewring translation).*

For thousands of years, we've tried to prophesize what the future will hold.  We hope that whatever visions come to us are truthful, but those are only confirmed when the significance of the event has already passed.  As in the passage above, Odysseus' men were similarly concerned with false visions of the future.  

As artificial intelligence continues to be researched and integrated into our daily lives, we should be as concerned as Odysseus' men with which visions of the future are true and which are false.

A neural network model approximates the way our actual brains work.  Information passes from one layer of neurons to the next, and the information gained from those interactions informs the final prediction.  Keras is a deep neural network library that makes use of TensorFlow on the backend to allow for easier and faster implementation.  Credited as its primary author, François Chollet, a Google enigeer, explained that the origin of the Keras name comes from the Greek word κέρας for horn.  It references imagery from the Odyssey where men are deceived by dream spirits who arrive through gates of ivory but are led to truth by spirits who arrive through gates of horn.  

Neural networks, in a single line, attempt to iteratively train a set (or sets) of weights that, when used together, return the most accurate predictions for a set of inputs. Just like many of our past models, the model is trained using a loss function, which our model will attempt to minimize over iterations. Remember that a loss function is some function that takes in our predictions and the actual values and returns some sort of aggregate value that shows how accurate (or not) we were.

Neural networks do this by establishing sets of neurons (known as hidden layers) that take in some sort of input(s), apply a weight, and pass that output onward. As we feed more data into the network, it adjusts those weights based on the output of the loss function, until we have highly trained and specific weights.

Why does one neuron turn out one way and a second neuron another? That's not generally something we can understand (though attempts have been made, such as Google's Deep Dream). You can understand this as a kind of (very advanced) mathematic optimization.

![neuralnet.png](/static/img/neuralnet.png)

What makes neural networks tick is the idea of hidden layers. Hidden does not mean anything particularly devious here, just that it is not the input or the output layer.

Hidden layers can have:

any number of neurons per layer
can be of any number in your model**
At each layer each neuron in that layer receives the same weight. However, each neuron is going to transform the data in a different way, based on how we assign or change the weights and bias in that neuron.

In the above example, there are XYZ hidden layers.

I applied Keras to the MSINT handwriting database made available by Kaggle to predict numbers based on their handwritten equivalent.  Below is a walkthrough of the steps I took to make predictions.


## Pre-Processing

To start, I read the training set into pandas, identifying 42,000 rows and 785 columns of data, inclusive of the target column, 'label.'  With the exception of the target column, all of the rest of the columns were pixel features of the images.  

Should I maybe talk about how to convert images to pixels here?


Because Keras requires a numpy array (as opposed to a pandas dataframe), I transformed the X set into a numpy matrix and then normalized the dataset by dividing each value by the maximum number of pixels (255).  I also transformed the target (y) values into a one-hot encoded matrix given that this dataset is a multi-classification problem.

Before feeding the data into the neural network model, I train-test-split my data for training purposes.


## Adding Layers

I initialized the model by making it Sequential() and adding layers to it.  Within a neural network, there are different layers that can be added to improve model performance.  First, I added a dense layer which connected all of the neurons.  I then added a "hidden" dropout layer.  A hidden layer does not mean that it is not present or not working - it just means that it isn't the initial input layer or the final output layer.  I decided to set my dropout layer to 0.5, which means that 50% of the neurons would be randomly "turned off" from training.  I did this to prevent the model from being overfit to the training set and to allow for some flexibility when faced with new data.

```
model = Sequential()
model.add(Dense(X_train.shape[1], input_shape=(784,), activation='relu'))
model.add(Dropout(.5))
model.add(Dense(y_train.shape[1], activation='softmax'))
```
Typically, we'll apply the weights in our neurons, add the bias and sum all terms, then pass that one value through the activation function.

Next, I activated the training set with 'relu' (Rectified Linear Unit) which only activates neurons whose output will be positive.  For the target, I activated with 'softmax' to essentially normalize the inputs within a scale of 0 to 1 to create a probability for an input falling within one of the target classes.

## Compilation



This is the section where I talk about using the 'adam' optimizer and where I also specify the loss metric.  For this particular muticlassification problem, I set the loss metric to 'categorical_crossentropy.'

When explaining why I chose the number of epochs I did, use this explanation:

Forward Propagation is straightforward -- either in batches or as individual observations, pass the training data through the network, applying all the weights, biases, and activation functions as usual. At this point, you should have actual and predicted values.

1.2.2.2  Backpropagation
What we want to do here is:

See how far off we were from the truth using the loss function
Identify which weights in our network are most responsible for how far we are off
Change all of the weights to make our model more accurate, changing the weights that are "the worst" the most
This is known as Backpropagation -- we are taking the errors we see in our model (as it stands currently) and are distributing them backwards to the rest of the layers.

What we'll do is train our data in a number of full passes known as epochs. As modelers, we'll choose a number of epochs to train our model, essentially choosing a value to stop where we see no additional change in the accuracy of our models.

The actual process is somewhat more complicated than that (see below), but the takeaway is backpropagation looks at how badly we did on each pass, moves those errors back up the model, and then uses gradient descent to change the weight over a series of iterations.

Do neural networks overfit? Yes, very much so. There are many weights to optimize and we can very quickly reach a point where the weights in each neuron are overfit to our training data (and therefore are limited in how well they predict new data).

## How to Prevent Overfitting

1.2.3.1  Regularization
Just like with linear models earlier in this course, we can also do regularization to make sure our weights are more generalizable. Because we're using our loss function to determine how we should change our weights, if we penalize the loss function to avoid larger weights, we will see the same behavior as we did with linear models -- weights will be large or impactful only if they contribute sufficiently to how well the model fits as a whole.


[TensorFlow playground](http://playground.tensorflow.org/#activation=relu&regularization=L1&batchSize=10&dataset=gauss&regDataset=reg-plane&learningRate=0.03&regularizationRate=0&noise=0&networkShape=2,1&seed=0.65075&showTestData=false&discretize=false&percTrainData=50&x=true&y=true&xTimesY=false&xSquared=false&ySquared=false&cosX=false&sinX=false&cosY=false&sinY=false&collectStats=false&problem=classification&initZero=false&hideText=false)
