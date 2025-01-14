---
title: "Performance"
layout: archive
author_profile: false
permalink: /strategy-performance/
---

### Strategy performance
What trades have I actually made recently? Is the strategy making money?  Is it making more than if I simply held the underlying shares and never sold them or any options? Here's a visual:
<figure>
    <a href="../images/transaction_history.png"><img src="../images/transaction_history.png"></a>
</figure>
<figure>
    <a href="../images/strategy_performance.png"><img src="../images/strategy_performance.png"></a>
</figure>

### Model performance
There are a few ways to measure the performance of the [predictive model](https://arkm97.github.io/covered-calls/volatility-model/).  One way is to compare the predicted probabilities for prices over some time window in the future and compare it to the observed prices over the same window.  Here is a histogram of the 10 day price history and the predicted probability density that was made 10 days ago:

<figure>
    <a href="../images/prediction_vs_outcome.png"><img src="../images/prediction_vs_outcome.png"></a>
</figure>
