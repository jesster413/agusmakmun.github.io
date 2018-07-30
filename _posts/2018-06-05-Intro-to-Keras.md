---
layout: post
title: The Writing's on the Wall, Part 1 (Intro to Keras)
categories: Neural-Network Keras Kaggle
published: False
---

Talk here about what a neural network is and some benefits to using a neural net over an XGBoost model, for example.


Talk here about Keras and who wrote it and maybe some history behind it.  Also, maybe talk about use cases.

Intro here about new dataset with handwriting and identifying number using MSINT.

## Pre-Processing

Talking about data pre-processing and the need to divide the values by the number of pixels.  

Talk about what a sequential model is.

Talk about dense and hidden layers.

Talk about compiling the model.



3  Preprocessing¶
When dealing with image data, you need to normalize your X by dividing each value by the max number of pixels (255).
Since this is a multiclass classification problem, keras needs y to be a one-hot encoded matrix




model = Sequential()

input_units = X_train.shape[1]
hidden_units = input_units

model.add(Dense(hidden_units, input_dim=input_units, activation='relu'))
model.add(Dense(1))

model = Sequential()
model.add(Dense(X_train.shape[1], input_shape=(784,), activation='relu'))
model.add(Dropout(.5))
model.add(Dense(y_train.shape[1], activation='softmax'))

model.compile(optimizer='adam', metrics=['accuracy'], loss='categorical_crossentropy')

from keras.optimizers import Adam

#model.compile(loss='mean_squared_error', optimizer='adam')

adam = Adam(lr=0.01)
model.compile(loss='mean_squared_error', optimizer=adam)
