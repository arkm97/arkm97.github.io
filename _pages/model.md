---
title: "Model"
layout: archive
author_profile: false
permalink: /volatility-model/
---

#### read about the model different ways: High-Level, no math (this page) | [More details, minimal math](https://arkm97.github.io/covered-calls/details-level-1/) | [All math](https://arkm97.github.io/covered-calls/details-level-2/)

### What does the model have to do?
The model has to generate a probability distribution for the price of the underlying at expiration.  With that distribution, calculating expected return and the variance of the return are straightforward.

### What am I modeling?
In short, I am making a prediction about people.  Participants in this sort of market are all essentially tasked with guessing at what price the other participants are willing to transact and where that price will be in the future.

Many factors may influence this guess (company fundamentals, news, performance of the broader market, etc.), but one stands out to me: price history.  This information is available to any participant, it incorporates the influence of many other factors, and I believe, it is what most participants are actaully using to make their guesses.  Here's the argument that convinced me: first, people's guesses are biased by memorable and readily available information.  We can't escape lingering pereptions that shares may continue to fall after a rough week, and it's dificult to resist finding patterns in a time series (pattern finding is human nature, after all).  Next, price history is the most reported and most memorable metric for goods that are traded. If you don't believe that last point, here's a rhetorical question: which do you find easier to recall for AAPL stock, P/E or share price? How about the P/E or share price 1 year ago? 10 years ago?  Finally, share price is inseparable from other predictors.  Big swings in share price influence news, which can lead to price moves, which in turn spawn more news, etc.  Strong share price performance can mean favorable financing or new growth initiatives, directly impacting fundamentals.  So, even if pariticipants use other metrics, they are (at least implicitly) making their guesses using the share price history.

With my model, I try to answer the following question: given the (recent) history of share price, what is the probability distribution for the share price at some point in the future?  Since I believe price is essentially driven by people's guesses about other people's guesses (and so on...), I am trying to model the _perceived_ distribution of participants in price space based on price history—guessing other people's guesses!—and then predict how that distribution will evolve.  

### Model Assumptions
Explicit assumptions are as follows:
- Buyers and sellers transact when they are "nearby" in price.  E.g., it is unlikely a transaction will happen if a seller wants \\$100 and a buyer is only willing to pay \\$50; it's likely they will transact if the buyer is willing to pay \\$99.
- Participants set their expectations based on expectations of others who are nearby in price.  If I want to buy at \\$100 and someone is willing to sell for \\$200, I probably won't move up my bid.  This participant is irrelevant to me.  If someone is asking for \\$110, I may change my bid.  This new participant is relevant to me, and changes my expectations, but only because he was nearby in price.
- Participants' expectations are sensitive to the (perceived) number of other participants willing to transact at a given price/time, and the rate of change of that number with both price and time.

An implicit assumption of my approach is that prices are not memoryless.  This is unconventional (more often, prices are modeled by [Martingales](https://en.wikipedia.org/wiki/Martingale_(probability_theory))), but I believe it aligns more closely with how people actually react to price changes.  

### Training
Given a functional form for the distribution ![P(X|\theta, data)](https://latex.codecogs.com/svg.latex?P%28X%7C%5Ctheta%2C%20%5Ctext%7Bdata%7D%29) with parameters ![\theta](https://latex.codecogs.com/svg.latex?%5Ctheta), I can construct a training dataset, then find ![\theta](https://latex.codecogs.com/svg.latex?%5Ctheta) that maximizes the likelihood of observing that data.  The training dataset comprises price observations over a window leading the prediction day ("inputs" to the model, along with ![\theta](https://latex.codecogs.com/svg.latex?%5Ctheta)), as well as observations lagging the prediction day (think of these as observed "outputs" of the model).  I used [MCMC](https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo) methods to model the posterior ![P(\theta|X, data)](https://latex.codecogs.com/svg.latex?P%28%5Ctheta%7CX%2C%20%5Ctext%7Bdata%7D%29), and selected the most likely ![\theta](https://latex.codecogs.com/svg.latex?%5Ctheta).

#### More details
There is a lot more to say about the assumptions I've made and how they lead to the form of the distribution ![P(X|\theta, data)](https://latex.codecogs.com/svg.latex?P%28X%7C%5Ctheta%2C%20%5Ctext%7Bdata%7D%29).  
[See how deep the rabbit hole goes...](https://arkm97.github.io/covered-calls/details-level-1)

#### How does the model perform?
See the [(live) performance of the model](https://arkm97.github.io/covered-calls/strategy-performance/)


<figure class="half">
  <a href="https://arkm97.github.io/covered-calls/strategy-performance/"><img style="width:200px" src="https://arkm97.github.io/covered-calls/images/prediction_vs_outcome.png"></a>
  <figcaption><a href="https://arkm97.github.io/covered-calls/strategy-performance/">Model performance</a></figcaption>
</figure>
