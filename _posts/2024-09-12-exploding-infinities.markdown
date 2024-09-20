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

There were a number of ways to solve this practically. Containerising applications makes sense if not constrained by the scale and complexity of the platform and the initial motivation to change machine types in the first place. Since OLS was used, we could have used SGD as the replacement solver given it's iterative nature. The trade-off would be an impact to the "understood performance" of the model and convergence time, however this would solve the issue of exploding values. 

Applying regularisation was another way. Introducing a bias term to the dataset could have stabilised the solver. There are many different regularisations that can be used like L1 and L2 biases. However, if one was only interested in attaining the most accurate model weights in the problem, the precision of these weights will be affected. This was the case our model was attempting to solve.

In the end, simply removing the infinities would not truly solve the nature of the problem that the model had set out to solve. Thus we turned to changing the methodology that would avoid our issue of numerical stability. Since we were dealing with an inverse optimisation problem, and given the non-linearity of the problem itself, non-convex solutions were finally investigated.

<br>

// Kudos to John and Debs when we watched F1 in the pub 🏎️
