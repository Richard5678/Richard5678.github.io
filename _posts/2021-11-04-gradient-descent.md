---
layout: post
title: Gradient Descent in Machine Learning
date: 2021-11-04 00:01:00
description: This article covers the implementation and theoretical framework of gradient descent. Topics such as feature scaling and cross-validation are also included to improve testing and training.
tags: math ml
categories: sample-posts
related_posts: false
---

This article covers the implementation and theoretical framework of gradient descent. Topics such as feature scaling and cross-validation are included to improve testing and training.  
Most of the theoretical work presented below is based on [Prof. Andrew Ng's notes](https://github.com/maxim5/cs229-2018-autumn/blob/main/notes/cs229-notes1.pdf).

---

## Theoretical Framework

The basic idea of gradient descent is to update the parameter $$\theta$$ toward the steepest direction of the cost function $$J(\theta)$$.  
Its update rule is given by:

<div class="d-flex justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/grad/grad1.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

Where $$\alpha$$ is the learning rate that determines the size of each update step.

The loss function $$J(\theta)$$ measures how well our model fits the dataset. A common choice is the **mean squared error (MSE)**, which is given by:

<div class="d-flex justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/grad/grad2.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

The additional 2 in the denominator simplifies the derivative later. Our task becomes minimizing $$J(\theta)$$.  
Taking the partial derivative of $$J(\theta)$$ gives:

<div class="d-flex justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/grad/grad3.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

Hence, the update rule becomes:

<div class="d-flex justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/grad/grad4.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

---

### Matrix Form Update Rule

The update rule described above updates entries in $$\theta$$ one at a time, making it messy to implement. Let us see how we can perform the update rule simultaneously for all entries of $$\theta$$ using matrix multiplication.

Define the design matrix $$X$$ as:

<div class="d-flex justify-content-center">
    <div class="col-sm-7 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/grad/grad5.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

Where each row is a training example. Observe that:

<div class="d-flex justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/grad/grad6.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

This completes the theoretical background needed to implement the gradient descent algorithm.

---

## Implementation

Here is an implementation of gradient descent for linear regression:

<div class="d-flex justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/grad/grad7.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

---

### Feature Scaling

To ensure proper updates, the matrix $$X$$ needs to be scaled. Each element in each column of $$X$$ is updated as follows:

<div class="d-flex justify-content-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/grad/grad8.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

---

## Cross-Validation

After fitting the model using gradient descent, the next step is to test its performance. A common method is **cross-validation**, which involves training the model on a fraction of the data and testing it on the remaining data.

**K-Fold Cross-Validation**  
A more advanced approach is k-fold cross-validation, where the dataset is divided into $$k$$ subsets. The model is trained on $$k-1$$ subsets and tested on the remaining subset. This process is repeated $$k$$ times, and the average testing result is calculated.

Here is an implementation:

<div class="d-flex justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/grad/grad10.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
