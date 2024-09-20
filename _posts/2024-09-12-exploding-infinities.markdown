---
layout: post
title:  "Exploding Infinities [Regression] [Machine Learning]"
date:   2024-09-12 08:53:16 +0100
categories: jekyll update
---

When working on large-scale machine learning (ML) platforms with simplified workflow interfaces for machine learning scientists, many model implementations can become black boxes. I once worked on a particular ML platform where several linear regression models had begun producing model weights to the order of 12 significant figures, which then were subjected to an np.exp() producing infinities, and the only thing changed had been the Databricks instance type it was running on! 

###### Diving into the cause

Initially I thought it was a difference in how random seeds defaulted based on the internal implementation we used for regression. Fixing it did not work. I began injecting logs throughout the system, trying to understand the data at each step of the flow, perhaps the data IO step corrupted the model inputs or there was a division of zero happening due to numerical precision? Only at the modelling step I discovered the models had extremely high condition numbers!

The workflow was using an internal component that transformed the dataset to keep only a large group of one hot encoded features to predict the target labels. The result was a highly sparse, 300+ column dataset with a significant portion of duplicated rows corresponding to a different label each. The models were solved via OLS, the common implementation calculates the pseudo-inverse of the matrix, thus attempting to solve an extremely ill-conditioned matrix naturally produced numerical instability down to machine level precision in its calculations - hence our exploding infinities!

What might have worked on one machine type, wouldn't necessarily always work on another, this becomes problematic when workflows aren't properly containerised making models effectively dependent on the machine architecture. From an engineering perspective, this also reduces the number of infrastructure options available when designing cost efficient ML systems and pipelines.

###### Controlling the explosion

There were a number of ways to solve this practically. Containerising applications makes sense if not constrained by the scale and complexity of the platform and the initial motivation to change machine types in the first place. Since OLS was used, we could use SGD instead. The trade-off would be an impact to the "understood performance" of the model and convergence time. However, simply removing the infinities would not truly solve the nature of the problem. 

Applying regularisation was another way. Introducing a bias term to the data could stabilise the solver. There are many different regularisations that can be used like L1, L2 biases. However, if one is optimising for accurate model weights, the precision will be affected.

In the end however, changing the methodology made most sense. It was crucial to understand what the model was attempting to solve, in this case we were dealing with an inverse optimisation problem. As with most real-world matrix problems, assuming linearity may never be the optimal approximation. We could look to use non-convex techniques, a widely researched field, in this example instead.

<br>

// Kudos to John and Debs when we watched F1 in the pub 🏎️
