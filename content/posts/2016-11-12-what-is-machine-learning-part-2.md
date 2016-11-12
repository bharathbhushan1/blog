---
title: "What is Machine Learning: Part 2"
date: 2016-11-12T10:00:00
draft: false
tags: ["machine learning"]
---

> Arthur Samuel described it as: "the field of study that gives computers the ability to learn without being explicitly programmed."

Basically the computer can be taught to do a particular task and once the learning is complete the computer can do that task well on its own. So the programming was not task specific, only the learning process was task specific. Something like public key encryption - the algorithms are public but the key ensures confidentiality and integrity.

> Tom Mitchell provides a more modern definition: "A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E."

This definition expresses a very clear dichotomy between learning and usage of the learning (scoring?). During the training phase of a machine learning application, it uses the input data to increase its knowledge of the problem/solution. During this time its experience E is increasing. If during that period its performance improves then it is machine learning.

For example if we write a digit classification problem which always predicts 1 for any input image, and we feed a random sequence of digits to it, it will start with a performance of 10% but will never improve. So no learning. Same is the case for a program which generates a random number between 0–9 and outputs that as its prediction for a given input.

But a true learning program would start with some small performance and as it sees each image, its performance would improve. Basically if the same sequence of images is repeated its performance will be strictly better. So there is machine learning.

But there is also a concept of generalization. Once the learning process is complete, the program's performance might deteriorate. I am not sure how the definition covers this aspect. I guess the definition does not prescribe an end to the learning; so continuously updating the experience should be fine.
