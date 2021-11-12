---
title: "Model"
layout: archive
author_profile: false
permalink: /volatility-model/
---

### What does the model have to do?
The model has to generate a probability distribution for the price of the underlying at expiration.  With that distribution, calculating expected return and the variance of the return are straightforward.

### What am I modeling? 

### Model Assumptions
Explicit assumptions are as follows:
- Buyers and sellers transact when they are "nearby" in price.
- Participants set their expectations based on interactions with others who are nearby in price.
- Participants' expectations are sensitive to the number of other participants willing to transact at a given price/time, and the rate of change of that number with both price and time.

An implicit assumption of my approach is that prices are not memoryless.  This is unconventional (more often, prices are modeled by [Martingales](https://en.wikipedia.org/wiki/Martingale_(probability_theory))), but I believe it aligns more closely with how people actually react to price changes.  

### Training
Given a functional form for the distribution ![P(X|\theta, data)](https://latex.codecogs.com/svg.latex?P%28X%7C%5Ctheta%2C%20%5Ctext%7Bdata%7D%29) with parameters ![\theta](https://latex.codecogs.com/svg.latex?%5Ctheta), I can construct a training dataset, then find ![\theta](https://latex.codecogs.com/svg.latex?%5Ctheta) that maximizes the likelihood of observing that data.  The training dataset comprises price observations over a window leading the prediction day ("inputs" to the model, along with ![\theta](https://latex.codecogs.com/svg.latex?%5Ctheta)), as well as observations lagging the prediction day (think of these as observed "outputs" of the model).  I used [MCMC](https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo) methods to model the posterior ![P(\theta|X, data)](https://latex.codecogs.com/svg.latex?P%28%5Ctheta%7CX%2C%20%5Ctext%7Bdata%7D%29), and selected the most likely ![\theta](https://latex.codecogs.com/svg.latex?%5Ctheta).

### Predictions

[see how deep the rabbit hole goes...](https://arkm97.github.io/covered-calls/details-level-1)

updated
