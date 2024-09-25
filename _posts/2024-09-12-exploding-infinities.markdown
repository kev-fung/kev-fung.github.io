---
layout: post
title:  "When machine learning models break: A lesson in numerical instability [Regression] [Machine Learning]"
date:   2024-09-12 08:53:16 +0100
categories: jekyll update
---

In large-scale machine learning platforms, one may encounter heavily abstracted workflows where model implementations and methods become opaque to the researcher. I once worked on such a platform where linear regression models started producing weights on the order of 10^12 figures, leading to sudden infinities after it applied an np.exp() to every weight. The only change I made was the EC2 instance type the workflow ran on!

###### Diving into the cause

Initially I thought it was a difference in how random seeds defaulted based on the internal implementation we used for regression. Fixing it did not work. So I injected logs throughout the system, trying to understand the data at every step of the flow - perhaps the data IO step corrupted the model inputs, or a division by zero occurred due to numerical precision? It wasn’t until the modelling step that I discovered the models had extremely high condition numbers!

The workflow was using an internal component that transformed the dataset by retaining a large group of one-hot encoded features for predicting the target labels. The result was a highly sparse, massive columnar dataset with many duplicated rows (each associated with a different label!). The models were solved via OLS, where its implementation calculated the pseudo-inverse of the matrix - thus it was attempting to solve an extremely ill-conditioned matrix and this was naturally producing numerical instability down to the machine level precision in its calculations! When numerical instability as extreme as this occurs, even changing the machine type can have unintended consequences.

The objective of the workflow was to optimise the weights for every combination of categorical variables through its regression models, rather than predict the label data it was given - we were effectively solving a model inverse problem.

###### Controlling the explosion

There were several practical solutions to tackle this issue. One approach was to properly containerise the application, which would ensure consistency across different machine types as long as the platform's scale and complexity allowed for it. Since we were using OLS, switching to an iterative solver like SGD could have resolved the exploding values. While this might have impacted the model's perceived performance and increased convergence time, it would at least have provided more stability.

Another option was to apply regularization by introducing a bias term to stabilize the solver. Techniques like L1 or L2 regularization could have helped, although they would have sacrificed some precision in the model weights. Accuracy was crucial for our specific use case, so this trade-off required careful consideration with the researchers.

What you should realise by now is that despite the possible mathematical and computational solutions to remove the infinities, the longer term (and probably wisest) decision was to reconsider the methodology again. The business problem the workflow was constrained to was definitely a hard problem to solve - however, this experience has reminded me of the importance of numerical stability (and fundamentally how numerical models are solved) when building machine learning solutions and it can often become the forgotten part of an ML practitioners toolkit.

<br>

// Shout out to John and Debs for the discussions we had over this topic 🏎️
