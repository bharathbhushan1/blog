---
title: "Decision Trees"
date: 2019-04-22T10:00:00
draft: false
tags: ["machine learning", "decision tree"]
---

This is a short post on decision trees based on my reading of Chapter 6 of [Hands–On Machine Learning with Scikit–Learn and TensorFlow ](https://www.amazon.in/Hands-Machine-Learning-Scikit-Learn-TensorFlow/dp/1491962291). I am just listing out some interesting points. The book is really good; so please refer to it.

### Basics
* Decision trees do not require much data preparation (feature scaling, centering etc).
* Gini score indicates impurity of a decision tree node (do all training examples belong to a single class or not)
* CART algorithm to produce a decision tree produces binary trees. ID3 produces trees whose nodes can have more than 2 children.
* Decision trees are white box models because it is easy to see why the decisions were made. Neural networks / Random forests are black box because of the lack of interpretability. 
* Decision trees are very easy to apply manually.

<!--more-->

### CART
* CART algorithm (Classification and Regression Trees) is used to create decision trees. 
* It splits the training set into two based on (f, fx) where f is a feature and fx is a threshold/discriminative value of that feature. Then it proceeds recursively in the same way in both the subsets.
* The choice of (f, fx) is based on minimising the produce of fraction of instances in the set and its Gini measure.
* Prediction is very fast and independent of the number of features.

### Regularisation parameters
* Decision trees can overfit since they are non-parametric (number of parameters is not decided before training starts).
* Hyperparameters like max_depth help in constraining the tree, thus regularising the model.

### Regression
* The tree predicts a value instead of a label. This value is the average value of that feature for that leaf's instances.
* MSE is to be minimised.

### Instability
* Decision boundaries are perpendicular to an axis. So the tree is sensitive to rotation (for example 45 degree rotation).
* For sample training data the trees can be different. 
* Also removing a small number of samples can change the model drastically.
