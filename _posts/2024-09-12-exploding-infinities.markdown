---
layout: post
title:  "When Machine Learning Models Break: A Lesson in Numerical Instability [Regression] [Machine Learning]"
date:   2024-09-12 08:53:16 +0100
categories: jekyll update
---

In large-scale machine learning platforms with simplified workflows, model implementations can become black boxes. I once worked on such a platform where linear regression models started producing weights in the magnitude of 10^12 figures, leading to infinities after applying np.exp(). The only change? The Databricks instance type!

###### Diving into the cause

Initially I thought it was a difference in how random seeds defaulted based on the internal implementation we used for regression. Fixing it did not work. I injected logs throughout the system, trying to understand the data at each step of the flow, perhaps the data IO step corrupted the model inputs or there was a division of zero happening due to numerical precision? Only at the modelling step I discovered the models had extremely high condition numbers!

The workflow was using an internal component that transformed the dataset to keep only a large group of one hot encoded features to predict the target labels. The result was a highly sparse, massive columnar dataset with many duplicated rows (and each with a different label!). The models were solved via OLS, where its implementation calculates the pseudo-inverse of the matrix - thus attempting to solve an extremely ill-conditioned matrix was naturally producing numerical instability down to machine level precision in its calculations.

###### Controlling the explosion

There were several practical solutions to tackle this issue. One approach was containerizing the applications, which would ensure consistency across different machine types, as long as the platform's scale and complexity allowed for it. Since we were using OLS, switching to an iterative solver like SGD could have resolved the exploding values. While this might have impacted the model's perceived performance and increased convergence time, it would have provided more stability.

Another option was to apply regularization, introducing a bias term to stabilize the solver. Techniques like L1 or L2 regularization could have helped, though they might sacrifice some precision in the model weights. Given that accuracy was crucial for our specific use case, this trade-off would need careful consideration.

In the end, the solution wasn’t just removing infinities. We had to rethink the methodology, focusing on non-convex solutions to handle the inverse optimization problem and improve numerical stability.

<br>

// Kudos to John and Debs when we watched F1 in the pub 🏎️
