---
layout: post
title:  "Skydiving and Risk"
date:   2022-04-20 17:27:18 -0700
categories: jekyll update
---
## Introduction

This post gives a simple risk assessment of skydiving and other such activities that come with added risk of death. This post is not a recommendation; I am not a doctor. But I think it's useful to think about risk in a principled way.


Let's start by quantifying risk of death in terms of micromorts, where 1 micromort is equal to $10^{-6}$ probability of death (micro is $10^{-6}$, mort is spanish for death). From wikipedia, below are micromort numbers associated with travel:

- Travelling 1000 mi by motorcycle: ~170
- Travlling 1000 mi by walking: 60
- Travelling 1000 mi by car: 4
- Travelling 1000 mi by jet: 1


By comparison, below are the numbers for some extreme sports:

- Skiing: 0.7 per day
- Marathons: 7 per run
- Skydiving: 8 per jump
- BASE Jumping: 430 per jump
- Mountaineering (Ascent to Matterhorn): ~2800 per ascent attempt
- Mountaineering (Ascent to Everest): ~38,000 per ascent attempt

Finally, for perspective here are some miscellaneous risks:

- Ecstasy: 0.5 per tablet
- All causes: ~20 per day
- Non-natural causes: 1.6 per day

## Analysis

The last figure is interesting as it suggests that risk on the order of magnitude of $10^{-6}$ is unavoidable in our every day lives. If we look closer into how these figure are calculated for a particular classification, they just divided the total deaths per day for that classification by the total population. For instance, for all causes, they divided total mortalities per day in the US by the population of the US.

$$2*10^{-5} \approx (2.5 M / 365)/(300M)$$

Thus, these risk numbers are empirical averages of risk of death gathered over hundreds of thousands of samples. To gauge how accurate these averages are, we can consider the variance of these empirical estimates, which we can view as yes-no random variables (Bernoulli).

The variance of a bernoulli random variable with true parameter $$p$$ is $$p(1 - p)$$, and the variance of an empirical estimator $$\hat{p} = \sum_{i = 1}^n \frac{1}{n} X_i$$ is $$Var(\hat{p}) = \frac{p (1 - p)}{n} \leq \frac{p}{n}$$. By making some mild assumptions about $$p$$, such as $$p < 0.01$$, we can compute that the square root of the variance ("standard deviation") is also on the order of $$10^{-6}$$.

From these estimates for mean and square root variance, we can construct an upper and lower bound for the true value of these statistics as $$mean + c * std$$ and $$mean - c * std$$, where c is a small constant. 

Using this upper confidence bound analysis, it seems that the "background" risk is on the order of $$10^{-6}$$ with high probability. In contrast, the high probability upper bound for risk of skydiving is

$$8*10^{-6} + \sqrt{\frac{0.01}{50M}} = 22 * 10^{-6}$$

Thus, with high confidence, the risk of skydiving is an order of magnitude higher than the "background" risk we encounter every day. Going on one skydive equals the risk of living 10-20 days.

From these numbers, it seems that the average risk of skydiving is relatiely mild. Just going by the averages listed (without any confidence analysis), one skydive is about the same risk as of driving from Seattle to LA and back. 




## Afterword

I think it's useful to estimate high confidence upper bounds on risk of activites you are considering trying, but ultimately you'd still have to make a judgement call on whether you think the risk you derived is too high for your liking. In my view, the choice of threshold should not be too much higher than the background risk. There's a sentiment on wallstreet that you should never make bets that could lose you your entire portfolio, even if you expect the bet to be positive on average ("never go all in"). I think a similar idea for catastrophic-risk minimization applies here, except that here your minimal risk attained is that which is on the order of magnitude of the background risk.

<!---

This blog post will be about risk--asking how much excess risk we need to accept (if any) in order to live fulfillingly. I'll start by talking about my assessment of skydiving. Note that I am not a doctor, and whatever I write here is not a recommendation for how you live your life.

## Risk Assessment of Skydiving

I'm really glad I skydived as I always wanted to know what it was like to fly. Of course, I'm speaking as someone who sustained no injuries, and the survivorship bias is a real consideration when reasoning about risky activities. If I'm someone who has never skydivied, it's not clear how much weight to give to the opinion of someone who's jumped before, or someone who's jumped hundreds of times ("maniacs" huh). 

Before we try to address this question rigorously, let me make a claim that is based on my intuition and experience. I think that for most people skydiving seems riskier than it actually is because it's unfamiliar. Images of falling to ones' death come to mind, and it's hard to gauge how likely that risk really is if skydiving isn't something you do often. On the other hand, you don't perceive much risk when you drive places because it's something you do regularly. You've driven hundreds of times and maybe gotten in an accident or two, but you know what to expect. We treat skydiving and driving differently because we are uncertain how likely it is that really bad things happen in the former. There is a large perceived variance in how badly the jump can go, which deters us from trying. 

To make our reasoning about potentially-fatal activities precise, let's quanitfy risk of death in terms of micromorts, where 1 micromort is equal to $10^{-6}$ probability of death (micro is $10^{-6}$, mort is spanish for death).

Below are micromort numbers associated with travel:

Travelling 1000 mi by motorcycle: ~170
Travlling 1000 mi by walking: 60
Travelling 1000 mi by car: 4
Travelling 1000 mi by jet: 1



By comparison, below are the numbers for some extreme sports:

Skiing: 0.7 per day
Marathons: 7 per run
Skydiving: 8 per jump
BASE Jumping: 430 per jump
Mountaineering (Ascent to Matterhorn): ~2800 per ascent attempt
Mountaineering (Ascent to Everest): ~38,000 per ascent attempt

Finally, for perspective here are some miscellaneous risks:

Murder: 10 per year
Ecstasy: 0.5 per tablet
All causes: ~20 per day







If we drove every day and admitted risk ____ then total risk for a year is, by union bound, ...



On the other hand, the variance of risk accrued is related as 1/n ....


Thus, though the average risk of driving is higher, we perceive it as less risky since its variance is lower. It is as if we measure risk based on upper bound of the risk, mean + var, like UCB algorithms.

## Risk that's worth vs Risk that's just crazy



## Fulfillment and Meaning

--->