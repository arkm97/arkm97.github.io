---
title: "More details"
layout: archive
author_profile: false
permalink: /details-level-1/
---

The first step is to estimate the distribution of participants in price space (more precisely, I'm estimating the distribution of _guesses_ about future distribution of participants in log return space).  Taking a window lagging the current time, I estimate this distribution using the price history. 

<img width="450" alt="price_history_hist" src="https://user-images.githubusercontent.com/22861412/149853509-cf541a17-06af-44ce-942b-dde618da7b70.png">

I'm making a number of assumptions here: there are participants where there are transactions; prices where there are many guesses show up more frequently in the price history; participants don't consider volumes or use any weighting when making a guess about the distribution.  

Next, I want to evolve that distribution forward in time so I can make a guess about the future.  Think of this evolution like a function that takes the current distribution and produces a new one (the output will be roughly the same bell shape as the initial distribution, but with the width, peak(s), and tails slightly transformed; derivation [here](https://arkm97.github.io/covered-calls/details-level-2/)).  There are unknown parameters in this function that determine e.g. how much the initial width matters, how sharp the in the output are, etc.  Based on many observations of initial and final distributions, I can find parameters that mazimize the likelihood of making those observations.  

The really interesting part comes when I try to incorporate the influence of the market at large.  I assume the same procedure, and estimate the distribution of participants in "expected return" space, i.e what participants want to get from any hypothetical investment.  Then, adapting the evolution function so I can use this distribution as another input/output, I do the same parameter-fitting procedure.  Rather than trying to use data across many markets for these observations, I do the following: I assume people are either 1) participating in the market of interest (the one where the underlying shares trade) or  2) not participating.  By this definition, all people in my model universe are in one of the groups...at least in the short run).  Then, I make a guess about how many have moved from 1) to 2) by comparing initial and final distributions for the market of interest (this is non-trivial.  The information I want is actually related to the difference in the distribution of spatial frequencies...this leds to an interesting observation: I'm _not_ modeling movement of some quantity of people, but an abstract quantity related to the information available to people.  For intuition about how spatial frequencies can be useful, imagine the surface of a lake on a calm day; compare that to the surface during a storm;  the same amount of water may be there, but there is certainly another energy-like quantity that is absent on the calm day whose presence on the stormy day I can infer by looking at the size of the waves.  This inference about the change in total "energy" is what I'm after).  With guesses about the final distributions both in and out of the market of interest, I can proceed with the parameter-fitting procedure as usual.

<img width="961" alt="interaction" src="https://user-images.githubusercontent.com/22861412/149853529-d0b20320-fb0a-4d2b-ab65-3985bc9a6df8.png">


<img width="1118" alt="sampling" src="https://user-images.githubusercontent.com/22861412/149853534-05c6444f-3c7e-4743-a7bc-1acd45748075.png">


<img width="627" alt="8-pt interaction" src="https://user-images.githubusercontent.com/22861412/149853543-a5485828-631d-41e2-8f86-a3a9232a528f.png">


<img width="573" alt="model scoring" src="https://user-images.githubusercontent.com/22861412/149853554-25f59d16-7393-41b8-abcc-4cbcf73cb79d.png">


<img width="566" alt="reconstruction of pdf" src="https://user-images.githubusercontent.com/22861412/149853567-76f1f15c-53ee-4abf-b65e-81db93c5c438.png">


<img width="468" alt="model output" src="https://user-images.githubusercontent.com/22861412/149853572-025c11e0-3be4-4e3e-a3b2-6a9ca8029062.png">


![Likelihood](https://user-images.githubusercontent.com/22861412/149853595-359b5674-ff30-417a-a29a-c22cc0e9e4c6.png)


![MCMC illustration](https://user-images.githubusercontent.com/22861412/149853609-7464939a-c464-4498-b154-6e485d3b5507.png)


Want even more math? [Take me to Wonderland](https://arkm97.github.io/covered-calls/details-level-2/)

Leave the rabbit hole? [Get me out!](https://arkm97.github.io/covered-calls/volatility-model/)

