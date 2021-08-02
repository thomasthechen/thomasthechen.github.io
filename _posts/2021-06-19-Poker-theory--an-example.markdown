---
layout: post
title:  "Poker Theory – an example"
date:   2021-06-19 17:27:18 -0700
categories: jekyll update
---
## Introduction

Poker is an _imperfect information_ game, which means that all the information that determines the state of the game is not known to all players (e.g. players don't know each others' hole cards). This makes poker different from perfect-information games, such as Tic-tac-toe, chess, or Go, where both players see the game state (the board) with perfect information of where everything is.

In poker, each player only sees their cards, both players' actions, and the community cards (their information set), but they can't be sure exactly which game state they are in since they don't know the other players cards _(does my opponent have the nuts or just a sandwich?)_. Intuitively, incomplete information is the reason concepts like _bluffing_ emerge in poker, but not in chess.

Due to this nature, to formally describe what is optimal in poker, we need a few concepts from game theory:

- expected value (EV)
- best response (BR)
- exploitability
- nash equilibrium (NE)

as well as some useful poker-specific concepts:
- ranges
- polarization
- balancing bluffs

We will illustrate what these concepts are with the following simplified game:

## A Simple Game

`Definition 1.1 (Poker River Game):` There are two players, P1 and P2. To start, there is $100 in the pot and both have a $100 stack. P1 goes first, choosing either to check (x) or to bet (b) any amount. Action moves onto P2, who can check (x) or bet (b) if checked to. P2 can fold (f), call (c), or raise (r) if facing a bet.  Action continues in this way, until either one player folds or calls, and in the latter case the player with the better hand wins (showdown).

To begin, because of the inherent uncertainty about what cards either player may have, we need the notion of a _range_, the set of possible hands a player may have, along with what probability they have it. More formally,

`Definition 1.2 (Poker Range):` For a given player A, let $$\Omega$$ be the set of hands A can have. Then a range is a probability distribution, $$f$$ over A.

We will study the Poker River Game where, at the start of the game, P1 has range $$ \{AA : 0.5, QQ : 0.5\} $$ (i.e. AA or QQ with equal probability), while P2 has range $$\{KK : 1.0\}$$. 

`Example 1.3 (P1 bets everything strategy):`

Suppose P1's strategy was to bet 100% of her hands, an amount $$x \leq 100$$. Then against P1's betting range, $$ \{AA : 0.5, QQ : 0.5\} $$, P2 can win 50% of the time against QQ (winning $100 + $100), and lose the other 50% of the time against AA (losing $100). P2's best response (BR) strategy is to call 100% of her hands, yielding an expected value of 

$$EV_2 = 0.5 (200) + 0.5 (-100) = $50$$

Because this game is a zero-sum game, it must be that

$$EV_1 + EV_2 = pot = 100$$. Hence

$$EV_1 = 50$$. In this way, we've calculated the EV's of a P1 strategy, and its P2 best response. But in fact, P1 can do better. We claim the following:

`Claim 1.4 (P2's strategy when checked to):` When checked to, P2 will check 100%.

_Proof:_ Represent P1's checking range as $$ \{AA : 1 - p, QQ : p\} $$. Against this range, if P2 were to check 100%, she gets an EV of 

$$EV_{2, x} = (1 - p) (0) + p (100) = 100p$$.

On the other hand, the EV for betting is lower. If she bet an amount x, P1's BR versus her bet is to fold 100% of her QQ and call 100% of her AA. Thus, the EV of betting is:

$$EV_{2, b} = (1 - p) (-x) + p (100) = 100p - x(1 - p) \leq EV_{2, x}$$.

Thus, P2 will always check back when checked to, due to the _polarization_ of P1's range (having hands stronger and weaker than P2's range). **Important Concept 1**

`Claim 1.5 (Nash Equilibrium for Poker River Game):` P1's nash equilibrium (NE) strategy is to bet 100% of her AA and 50% of her QQ, and check the other 50% of her QQ. P2's NE strategy, when facing a bet, is to call 50% and fold 50%; and facing a check, P2 checks 100%. For this NE, 
$$EV_1 = 75, EV_2 = 25$$

_Proof:_ Suppose, first, that P1 checks to P2. By Claim $$1.4$$, P2 will always check back. Therefore, since AA is guaranteed the best hand, P1 will always bet 100% of her AA since the EV of betting (where P2 might call) dominates the EV of checking (where P2 always checks back).

Now, let q be the fraction of P1's QQ that she bets, (for now, let her bet size be 100). Consider 2 cases:

1. q < 0.5. Then P1's (unnormalized!) betting range is $$ \{AA : 0.5, QQ : 0.5q\} $$. The EV of folding is 0. But the EV of calling is 

$$EV_{2, c} = 0.5 (-100) + 0.5q (200) < 0 = EV_{2, f}$$.

Thus, when facing a bet (w.p. 0.5 + 0.5q), P2 always folds, with EV 0. And when facing a check (w.p. 0.5 (1 - q)), P2 is always facing QQ, and hence always wins. Her EV in this line is 100. The EV of P2's strategy is then

$$EV_2 = (0.5 + 0.5q) * 0 + 0.5 (1 - q) * 100 = 50 (1 - q) \geq 25   (*)$$


2. q > 0.5. Then P1's (unnormalized!) betting range is $$ \{AA : 0.5, QQ : 0.5q\} $$. Facing this range, the scenario is similar to example 1.3 and P2's BR is to call 100% when facing a bet, and to check 100% when facing a check. It can be verified that P2's strategy's EV is

$$EV_2 = 150q - 50 \geq 25   (* *)$$


In either of these cases, as evident in equation $$(*) and (* *)$$, P1 can minimize P2's EV (hence maximizing her own EV) by setting q = 0.5. Thus, her strategy is to bet 100% of AA, bet 50% of QQ, and check the other 50%. This balance of her betting range makes the EV of P2's call and fold actions equal, making P2 indifferent to these actions. **Important Concept 2** With this strategy, she guarantees that P2's EV is at most 25, i.e. that her own EV is at least 75.


With P1 playing this strategy, P2's EV for calling or folding are equal. This means P2 can play either with any probability, right?

No. Suppose P2 called 100%. Then P1 can adjust her strategy to only bet with AA and check all QQ, yielding an EV of:

$$EV_1 = 0.5 (200) + 0.5 * 0 = 100 > 75$$

This is an example of P2 playing an _exploitable_ strategy. Even though against P1's NE strategy, she nets the same EV as her actual NE strategy (which is some mix of calling and folding), her strategy fares much worse in EV against a BR strategy, in this case P1 betting only AA. A further remark is that when P1 is playing a NE strategy and P2 is playing an exploitable strategy, P2's EV can be no higher than P2's EV using her NE strategy. That is, when P1 plays a NE strategy, the worst P1 can do is when P2 also plays a NE strategy. Any other strategy P2 plays can only be beneficial for P1's EV **Important Concept 3**.

Similarly, if P2 folded 100%, P1 would just bet 100% of QQ, netting the entire pot (EV = 100), while P2 gets EV of 0. 

There exists a mix of calling and folding that optimizes P2's EV, which you can easily verify to be calling 50% and folding 50%. Any deviation from this NE strategy (e.g. calling 55%) will allow P1 to exploit P2 (e.g. by bluffing less with QQ and pure-value-betting).

We conclude the proof of the NE claim here. P1 bets 100% of AA and 50% of QQ, and checks 50% of QQ. P2 calls 50% and folds 50%. $$EV_1 = 75$$ and $$EV_2 = 25$$.



## Commentary and Key Concepts

## Extension of Concepts to No-limit Texas Holdem



Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
