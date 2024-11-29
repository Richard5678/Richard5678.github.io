---
layout: post
title: Implementing Random Forest from Scratch in Python
date: 2021-12-07 00:01:00
description: This article is a walk-through of an implementation of random forest classifier without using any implementations from existing libraries.
tags: math ml
categories: sample-posts
related_posts: false
---


This article is a walk-through of an implementation of a random forest classifier without using any implementations from existing libraries.

---

## Motivation

I am recently studying different kinds of machine learning algorithms. The most effective way of learning different algorithms is to implement them concretely. With this in mind, I have previously implemented linear and logistic regressions, which are relatively simple models.

My next target is a type of decision tree algorithm. After some research, it became apparent that random forest and gradient boosting trees are the two most popular and effective tree-based algorithms. Unlike many other types of algorithms, like convolutional neural networks, there is very limited information about how these two algorithms are implemented in "easy-to-understand" code.

After implementing my own version of a random forest following weeks of struggle, I want to share my implementation and the lessons I have learned.

---

## What is Random Forest?

The simple answer is: Random Forest is a type of decision tree algorithm that uses bagging (training multiple trees).

A decision tree asks a question at each node and splits the data into two child nodes: one with examples that respond "yes" to the question and the other with examples that respond "no." The goal of a split is to group examples with similar labels together. For example:
- In regression, group similar target values together.
- In classification, group similar labels together.

To achieve this, we define a loss function that quantifies the quality of a split:
- **Regression Tasks:** Use Mean Squared Error (MSE).
- **Classification Tasks:** Use Gini Index (used in this example).

A Random Forest consists of multiple decision trees, each trained on a subset of features and training examples. At test time, it predicts by taking the majority class from all the trees in classification tasks.

---

## Implementation

### Steps for Training a Single Tree in Random Forest:
1. Randomly sample features and training examples.
2. Find the optimal split by calculating the loss for each possible value of the given feature.
3. Split the data at the current node based on the optimal split point.
4. Recursively build the left and right subtrees until the maximum depth is reached.

### Core Program Function: `random_forest`

The `random_forest` function calls the `build_tree` function multiple times (equal to the number of trees) and updates a progress bar. The resulting trees are stored in a list named `decision_trees`.

<div class="d-flex justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/random_forest/random1.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

---

### Recursive Tree Building: `build_tree`

The function `build_tree` constructs the decision tree recursively. Its base case triggers when:
- The maximum depth is reached.
- There are no features left to split.

**Key Sections in `build_tree`:**
1. **Finding the Best Split (Lines 41–49):**  
   Sample √(total features), then calculate Gini index for each possible split.

<div class="d-flex justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/random_forest/random3.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

2. **Calculating Gini Index:**  
   The Gini index formula is: 

<div class="d-flex justify-content-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/random_forest/random5.png" class="img-fluid rounded z-depth-1" zoomable=true width="500" %}
    </div>
</div>

   where \(p_i\) is the fraction of examples with label \(i\).

   A lower Gini index indicates a better split, as it ensures higher node purity.

3. **Construct Node (Lines 52–55):**  
   Initialize the node and store the split point.

4. **Build Subtrees (Lines 59–62):**  
   Recursively build left and right subtrees if:
   - Both sub-groups have examples.
   - The split improves the Gini index.

5. **Label Leaf Nodes (Lines 65–73):**  
   Assign the majority label to the leaf node.

<div class="d-flex justify-content-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/random_forest/random2.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

---

### Prediction and Evaluation

To predict on a new input \(x\), the `classify` function traverses the tree from the root to the appropriate leaf.

<div class="d-flex justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/random_forest/random8.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

To evaluate the model, an n-fold cross-validation is performed using the `eval` function.

<div class="d-flex justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/random_forest/random10.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

---

## Results

Using 10-fold cross-validation, the implementation achieves **86% accuracy** on the validation set.

<div class="d-flex justify-content-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/random_forest/random12.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

---

## Parameter Tuning

In Random Forest, two main hyperparameters must be set:
1. **Max Depth**
2. **Number of Trees**

Using the sklearn library for speed, we tested combinations of these parameters. The best results:
- 50 trees
- Max depth = 6  
  achieved **85.93% validation accuracy.**

<div class="d-flex justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/random_forest/random14.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

---

## Full Implementation

For the complete implementation, visit the [GitHub repository](https://github.com/Richard5678/Machine-Learning/blob/main/heart%20problem%20-%20random%20forest.ipynb).
