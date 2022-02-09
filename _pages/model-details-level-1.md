---
title: "More details"
layout: archive
author_profile: false
permalink: /details-level-1/
---
#### read about the model different ways: [High-Level, no math](https://arkm97.github.io/covered-calls/volatility-model/) | More details, minimal math (this page) | [All math](https://arkm97.github.io/covered-calls/details-level-2/)

The first step is to estimate the distribution of participants in price space (more precisely, I'm estimating the distribution of _guesses_ about future distribution of participants in log return space).  Taking a window lagging the current time, I estimate this distribution using the price history. 

<img width="450" alt="price_history_hist" src="https://user-images.githubusercontent.com/22861412/149853509-cf541a17-06af-44ce-942b-dde618da7b70.png">

I'm making a number of assumptions here: there are participants where there are transactions; prices where there are many guesses show up more frequently in the price history; participants don't consider volumes or use any weighting when making a guess about the distribution.  

Next, I want to evolve that distribution forward in time so I can make a guess about the future.  Think of this evolution like a function that takes the current distribution and produces a new one (the output will be roughly the same bell shape as the initial distribution, but with the width, peak(s), and tails slightly transformed; derivation [here](https://arkm97.github.io/covered-calls/details-level-2/)).  There are unknown parameters in this function that determine e.g. how much the initial width matters, how sharp the peaks in the output are, etc.  Based on many observations of initial and final distributions, I can find parameters that mazimize the likelihood of making those observations.  

The really interesting part comes when I try to incorporate the influence of the market at large.  I assume the same procedure, and estimate the distribution of participants in "expected return" space, i.e what participants want to get from any hypothetical investment.  Then, adapting the evolution function so I can use this distribution as another input/output, I do the same parameter-fitting procedure.  Rather than trying to use data across many markets for these observations, I do the following: I assume people are either 1) participating in the market of interest (the one where the underlying shares trade) or  2) not participating.  By this definition, all people in my model universe are in one of the groups...at least in the short run.  Then, I make a guess about how many have moved from 1) to 2) by comparing initial and final distributions for the market of interest (this is non-trivial.  The information I want is actually related to the difference in the distribution of spatial frequencies...this leds to an interesting observation: I'm _not_ modeling movement of some quantity of people, but an abstract quantity related to people's guesses and the information available to them.  For intuition about how spatial frequencies can be useful, imagine the surface of a lake on a calm day; compare that to the surface during a storm;  the same amount of water may be there, but there is certainly another energy-like quantity that is absent on the calm day whose presence on the stormy day I can infer by looking at the size of the waves.  This inference about the change in total "energy" is what I'm after).  With guesses about the final distributions both in and out of the market of interest, I can proceed with the parameter-fitting procedure as usual.

The whole process looks something like this:

<img width="961" alt="interaction" src="https://user-images.githubusercontent.com/22861412/149853529-d0b20320-fb0a-4d2b-ab65-3985bc9a6df8.png">

There is one distribution for everyone outside the market and one for everyone in it.  The initial distributions "interact" and produce two final distributions.  

While the end goal is to estimate a distribution, the model estimates transition probabilities from an initial to a final price (log return).  It is quite complicated to do this for an arbitrary number of participants.  Instead, I can ask a simpler question about only one participant, drawn from the initial distribution, and use it to estimate the final distribution.  That question is: "if I observe a participant transacting at price x now, how likely is it that I'll see the same participant transacting at price y later?"  In order to reconstruct a final distribution, I have to sample the initial distribution many times and sum up the transition probabilities for all possible y over all trials.  

In fact, if I want to model the interaction of participants with other participants, I need to sample (at least) two participants at once and answer the joint question: "if I observe participants transacting at x<sub>1</sub> and x<sub>2</sub> now, how likely is it that I'll see the same participants transacting at y<sub>1</sub> and y<sub>2</sub> later?"  (Further, if I want to model interactions inside and outside the market, I need two participants from each for a total of four.)  

Given an observation of initial distributions, I can choose any two points:

<img width="1118" alt="sampling" src="https://user-images.githubusercontent.com/22861412/149853534-05c6444f-3c7e-4743-a7bc-1acd45748075.png">

...let them "interact":
 
<img width="627" alt="8-pt interaction" src="https://user-images.githubusercontent.com/22861412/149853543-a5485828-631d-41e2-8f86-a3a9232a528f.png">

...calculate the likelihood of the transition:
 
<img width="573" alt="model scoring" src="https://user-images.githubusercontent.com/22861412/149853554-25f59d16-7393-41b8-abcc-4cbcf73cb79d.png">

...then sum the likelihoods to get an estimate for the final distribution.  (Note: I'm using "distribution" pretty loosely...but this model output is nonnegative and it can be normalized):

<img width="566" alt="reconstruction of pdf" src="https://user-images.githubusercontent.com/22861412/149853567-76f1f15c-53ee-4abf-b65e-81db93c5c438.png">


<img width="468" alt="model output" src="https://user-images.githubusercontent.com/22861412/149853572-025c11e0-3be4-4e3e-a3b2-6a9ca8029062.png">


The training procedure, where I set the parameters of the model, is pretty similar.  I perform the same sort of sampling and scoring for many possible pairs of initial/final samples across many different initial and final distributions (here, the final distributions are actually observed).  The set of parameters used in the model is assigned a likelihood.  

![Likelihood](https://user-images.githubusercontent.com/22861412/149853595-359b5674-ff30-417a-a29a-c22cc0e9e4c6.png)

Then, the final task is to find set of parameters that maximizes the likelihood of observing the actual data.  I used a [MCMC](https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo) procedure for this.  Here's how that goes: I start at some set of parameters, then take a random step in parameter space and accept the new values with a probability related to their likelihood.  The steps will tend toward more likely values for the parameters.  By assessing the size of the steps and the acceptance rate, I determine if the chain has "converged" on a region of high likelihood.  The trace of the chain gives the joint likelihood distribution for the parameters.  I pick the most likely set and fitting is complete!

![MCMC illustration](https://user-images.githubusercontent.com/22861412/149853609-7464939a-c464-4498-b154-6e485d3b5507.png)


Want even more math? [Take me to Wonderland](https://arkm97.github.io/covered-calls/details-level-2/)

Leave the rabbit hole? [Get me out!](https://arkm97.github.io/covered-calls/volatility-model/)

