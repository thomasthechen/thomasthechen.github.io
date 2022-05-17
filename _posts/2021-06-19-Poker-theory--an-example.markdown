---
layout: post
title:  "Poker Theory Principles with an Example"
date:   2021-06-19 17:27:18 -0700
categories: jekyll update
---
## Introduction  test

This article aims to explain the game theory principles in Poker, and to provide intuitive heuristics to applying these principles to improve your game.

Poker is an imperfect information game as players don't know each others' hole cards. In many ways, this makes poker harder than perfect-information games (where players see the entire game state), such as Tic-tac-toe and chess, because of the ability to bluff. 

Bluffing makes strategic considerations probabilistic. E.g. instead computing "what if Nxe4", a poker player considers "What are the chances my opponent raises if I bet, and with what hands?". To describe what is optimal in poker, we need the following game theoretic concepts:

- expected value (EV)
- best response (BR)
- exploitability
- nash equilibrium (NE)

as well as some useful poker-specific concepts:
- ranges
- polarization
- balancing bluffs

We will illustrate these concepts by providing a principled analysis of the following, simplified poker game:

## A Simplified Poker Game

`Definition 1.1 (Poker River Game):` There are two players, P1 (Alice) and P2 (Bob). To start, there is $100 in the pot and both players have a $100 stack. P1 goes first, choosing either to check (x) or to bet (b) any amount. If P1 checks, P2 can check (x), or bet (b). If P1 bets, P2 can fold (f), call (c), or raise (r). Action alternates in this way, until one player either folds or calls. In the latter case, the player with the better hand wins (showdown).

First, because of a given player receives limited information about the other player's possible hands, we need the notion of a _range_, to describe the possible hands their opponent could have, along with what probability they estimate their opponent to have it, based on the information they have. 

`Definition 1.2 (Poker Range):` For a given player A, let $$\Omega$$ be the set of hands A can have. Then a range is a probability distribution, $$p$$ over $$\Omega$$. Ranges are typically discussed in context of the the information set available: a player's private cards, the actions the players have made, and the public cards that have been dealt.

We will study the Poker River Game where, at the start of the game, P1 has range $$ \{AA : 0.5, QQ : 0.5\} $$ (i.e. AA or QQ with equal probability), while P2 has range $$\{KK : 1.0\}$$. 

`Naive Suboptimal Strategy 1.3 (P1 bets everything):`

Suppose P1's strategy was to bet 100% of her hands, for an amount $$x \leq 100$$. Against P1's betting range, $$ \{AA : 0.5, QQ : 0.5\} $$, P2 wins 50% of the time, when against QQ (winning $100 pot + $100 bet), and loses the other 50% of the time, when against AA (losing $100). With this, P2 has a simple counter strategy to win in expectation against P1: to call 100% of his hands. The expected value of P2's strategy is:

$$EV_2 = 0.5 (200) + 0.5 (-100) = $50$$

Because this game is a zero-sum game, it must be that

$$EV_1 + EV_2 = pot = 100$$. Hence, $$EV_1 = 50$$. In this way, we've calculated the EV's of a P1 strategy, and P2's best response. However, P1 can actually do better than an EV of 50. We claim the following:

`Claim 1.4 (Nash Equilibrium for Poker River Game):` P1's nash equilibrium (NE) strategy is to bet 100% of her AA and 50% of her QQ, and check the other 50% of her QQ. P2's NE strategy, when facing a bet, is to call 50% and fold 50%; and facing a check, P2 checks 100%. For this NE, 
$$EV_1 = 75, EV_2 = 25$$

To prove this, we first show the following fact:

`Claim 1.5 (P2's strategy when checked to):` When checked to, P2's optimal strategy is to check 100%.

_Proof of 1.5:_ Represent P1's checking range as $$ \{AA : 1 - p, QQ : p\} $$. Against this range, if P2 were to check 100%, he gets an EV of 

$$EV_{2, x} = (1 - p) (0) + p (100) = 100p$$.

On the other hand, the EV for betting is lower. If he bets an amount x, P1's best response (BR) versus his bet is to fold 100% of her QQ and call 100% of her AA. The EV of betting is:

$$EV_{2, b} = (1 - p) (-x) + p (100) = 100p - x(1 - p) \leq EV_{2, x}$$.

Thus, P2 will always check back when checked to, due to this _polarization_ of P1's range, where P1's range contains hands both stronger and weaker than P2. **Important Concept 1** Returning to the main claim,

_Proof of 1.4:_ Suppose that P1 checks to P2. By Claim $$1.5$$, P2 will always check back. Therefore, since AA is guaranteed the best hand, P1 will always bet 100% of her AA since the EV of betting (where P2 might call) dominates the EV of checking (where P2 always checks back).

Now, let q be the fraction of P1's QQ that she bets. For now, let her bet size be 100. Consider 2 cases:

_Case 1: q < 0.5_. Then P1's (unnormalized) betting range is $$ \{AA : 0.5, QQ : 0.5q\} $$. P2's EV of folding is 0. But the EV of calling is 

$$EV_{2, c} = 0.5 (-100) + 0.5q (200) < 0 = EV_{2, f}$$.

Thus, when facing a bet (w.p. 0.5 + 0.5q), P2 always folds, with EV 0. And when facing a check (w.p. 0.5 (1 - q)), P2 is always facing QQ and will always win. His EV in this line is 100. The EV of P2's strategy is then

$$EV_2 = (0.5 + 0.5q) * 0 + 0.5 (1 - q) * 100 = 50 (1 - q) \geq 25   (*)$$


_Case 2: q > 0.5_. Then P1's (unnormalized) betting range is $$ \{AA : 0.5, QQ : 0.5q\} $$. Facing this range, the scenario is similar to example 1.3 and P2's BR is to call 100% when facing a bet, and to check 100% when facing a check. It can be verified that P2's strategy's EV is

$$EV_2 = 150q - 50 \geq 25   (* *)$$


Examining equations $$(*)$$ and $$(* *)$$, P1 can minimize P2's EV (hence maximizing her own EV) by setting q = 0.5. Thus, her strategy is to bet 100% of AA, bet 50% of QQ, and check the other 50%. 

By "balancing" her betting range in this way, P1 makes the EV of P2's call and fold actions equal, making P2 indifferent to these actions. **Important Concept 2** Moreover, she guarantees that P2's EV is at most 25, i.e. that her own EV is at least 75.


With P1 playing this strategy, P2's EV for calling or folding are equal. This means P2 can play either with any probability, right?

No. Suppose P2 called 100% of the time. Then P1 can adjust her strategy to only bet with AA and check all QQ, yielding an EV of:

$$EV_1 = 0.5 (200) + 0.5 * 0 = 100 > 75$$

The above is an example of P2 playing an _exploitable_ strategy. Even though against P1's NE strategy, he nets the same EV as his actual NE strategy (which is some mix of calling and folding), his strategy fares much worse in EV against a BR strategy: P1 betting only AA. A further remark is that when P1 is playing a NE strategy and P2 is playing an exploitable strategy, P2's EV can be no higher than P2's EV using her NE strategy. That is, when P1 plays a NE strategy, the worst P1 can do is when P2 also plays a NE strategy. Any other strategy P2 plays can only be beneficial for P1's EV **Important Concept 3**.

Similarly, if P2 folded 100%, P1 would just bet 100% of QQ, netting the entire pot (EV = 100), while P2 nets an EV of 0. 

There exists a mix of calling and folding that optimizes P2's EV: calling 50% of the time and folding 50%. Any deviation from this NE strategy (e.g. calling 55%) will allow P1 to exploit P2 (e.g. by bluffing less with QQ and pure-value-betting).

Thus, at equilibrium, P1 bets 100% of AA and 50% of QQ, and checks 50% of QQ. P2 calls 50% and folds 50%. $$EV_1 = 75$$ and $$EV_2 = 25$$.



## Commentary and Key Concepts

We'll start by elaborating on the key concepts highlighted.

**Important Concept 3** If you play a NE strategy, the best your opponent can do against you is by playing a NE themself. In a heads up setting, if both players play a NE, then they both earn an EV of 0, and either player will only lose EV if they deviate from their NE strategy. This is what Game theory optimal (GTO) maximalists mean when they say that playing GTO cannot lose in expectation. 

In particular, if you and I are playing a lot of hands together (10,000+), the following two points become true.

1. we are able to realize the edge our strategies may have over our opponent. If I'm playing a NE and you aren't, you will likely be down money due to the law of large numbers.
2. Due to playing so many hands, we can gauge generally what the other person's strategy is: at each spot, how often are they betting, with what sizing, and with what hands?. This means that if you are playing a highly exploitable strategy, after 10,000 hands your oppoonent will catch on, adjust, and exploit your strategy.

Under these circumstances, as in online cash games, it is important to play a sound game-theoretic strategy. In the 25,000 hand heads up challenge between Doug Polk and Daniel Negreanu, the reason Polk was over a 4:1 favorite was because of the edge he has in playing a more GTO strategy.

However, in most poker settings, these assumptions don't apply. If you walk into a live cash game in your casino, the players at the table haven't played enough with you to know your strategy, so point 2 is invalid. They certainly aren't playing perfectly themselves, so point 1 is also invalid. Thus, instead of playing a GTO strategy, it is often more profitable to play an exploitative strategy, where you gauge what your opponent's exploitable tendencies are (e.g. betting 100% on the flop), and adjust your strategy to exploit it. 

However, learning GTO is crucial in being able to play stronger players and longer sessions. Having solid GTO fundementals can only help you as you play stronger and stronger players.


**Important Concept 1** When your range is _polarized_, your strongest hands are generally stronger than your opponent's hands, and your weakest hands are generally weaker than your opponent's hand. In our toy game, Player 1 had a perfectly polarized range while Player 2, conversely, had a condensed range.

We saw from claim 1.3 that it was highly suboptimal for Player 2 to bet into player 1. When Player 1 is polarized and facing a bet, she can always just fold her bad hands and call with her good hands. Against this simple counter strategy, P2's bet achieves nothing positive. 

More generally, when you bet, you have two objectives: get called by worse hands or fold out better. If you can do neither, as when your opponent is polarized, you must check. This is why in single raised pots (SRPs), action is usually checked to the preflop aggressor. On most flops, the aggressor has the most nutted hands in their range (Overpairs, top set, etc.), while the preflop caller has considerably fewer nutted hands, as they would have raised many of them preflop.

The more general takeaway is that your optimal strategy is a mainly dependent on certain features of your range and their range (and whether you are in position). In the special case where your range is polarized, betting is heavily incentivized as your garbage can fold out better and your nuts can get called by worse. 

Of course, in no-limit hold'em most spots aren't perfectly polarized as in the toy game. Estimating the optimal play requires a lot of intuition-building by playing spots and studying them afterwards. But here's two simplified heuristics that capture the most important range considerations when deciding when to bet and how much:

1. does my range have an equity advantage? That is, on average, are my hands stronger than my opponent's hands?
2. does my range have a nut advantage? That is, are my best hands stronger than my opponent's best hands?

Regarding 1, if you have an equity advantage, you should generally be betting more frequently. This is why in single raised pots, on a board like Ad Qh 3s, the preflop aggressor is betting close to 100% of range ("range betting"), often for a small size. Conversely, if you have a severe equity disadvantage, you should check more frequently.

Of course, equity isn't the sole consideration. Our toy game example was one where both players had equal equity: Half the time, player 1 had the best hand, the other hand player 1 had the worst hand. Yet, the optimal strategy was only for P1 to bet, and when she did, she netted more EV than player 2 did. 

This takes us to 2. If you have a severe nut disadvantage, you should check. Moreover, if you have the nut advantage, you should generally bet larger. You are less afraid to put a lot of money in the pot, since your opponent is under constant threat of being stacked by your best hands, while you aren't. 

Nut advantage usually informs your bet size, while equity informs your bet frequency. With that said, here are a few common scenarios:

1. Large range advantage, Large nut advantage (e.g. SRP preflop aggressor on A Q 3 rainbow flop). Bet large (2/3 pot) and bet frequently (close to 100% of range).

2. Large range advantage, small to no nut advantage (e.g. SRP preflop aggressor on Q J 10, monotone). Bet small (1/3 pot) and bet frequently.

3. No range advantage or slight range disadvantage, but decent nut advantage (e.g. SRP preflop aggressor bets on A Q 3 rainbow flop, the opponent calls, and the turn is a 4). Bet infrequently, but when you do bet large (this is known as polarizing the turn).

4. Slight range advantage, little to no nut advantage. Should consider checking a high frequency, especially when out of position. If you are going to bet, let it be in position, and for a small size and low frequency. (e.g. SRP preflop aggressor on 6 5 4 two tone).


**Important Concept 2** Indifference is an important concept to note when constructing your strategies. In our toy game, player 1 made player 2 indifferent to her options of calling and folding, by mixing in the correct number of bluffs in her betting range. It was at the precise point where player 2 was indifferent that player 1 earned max EV. If player 1 overbluffed, calling became preferred. If player 1 underbluffed, folding became preferred. 

When constructing your betting ranges in no limit holdem, you should have a target set of hands in mind that you want to make indifferent __or colloquially, to put in a tough spot__. It is in this exact scenario that they lose most EV theoretically, and moreover that in practice they make the most mistakes (minus EV decisions). When betting, think of the type of hand you want to target: the middling-type hand they will make a lot of mistakes with, facing your bet, and either over fold or overcall.



## A Framework for Poker Decision-making

When you have 10-20 seconds for each decision, it's important to have a structured decision making process that takes into account the most important information. Here is how I suggest doing that:

1. (Info Recap) Recap the important information. What actions you took, what actions they took, what cards came down, what position you are in (IP or OOP).

2. (Range Strategy) Make a rough estimate your range and their range, and ask yourself what your range wants to do in this spot. E.g. do I want to bet frequently and small, bet infrequently but large, etc. 

3. (Hand Strategy) Ask yourself what your hand wants to do. If I have a nutted hand or a strong draw, I want to bet large to polarize. If I have a middling hand, I want check to get to showdown for cheap, beause of fear of betting and getting raised. To this point, always consider what will happen for your hand if you take an action and your opponent puts in more aggression: can my middling draw or middling showdown-value hand withstand a check raise? If not, I should consider checking more.

4. (Estimating GTO Action) If your range allows you to bet, then decide whether your current hand should be in the betting range.

5. (Exploitative Considerations) Now consider any exploitative tells or tendencies of your opponent (e.g. if they always cbet flop with good hands, but here they check, then this signals extreme weakness). If you think your reads are strong enough, consider deviating from your GTO baseline. There are also a lot of good exploitative tells: bet sizing, bet timing (if they bet quickly in a spot they should be thinking hard about, they're probably bluffing), general demeanor (acting weak signals strength, and vice versa), and table talk.

6. Make your decision. If you still can't decide on what to do, lean toward the safer option.

## A Simple, but Solid Strategy

Credit to Doug Polk for coming up with this strategy.

Categorize your hands into 4 categories:

1. strong showdown hands (e.g. top pair)
2. medium strength showdown hands (e.g. middle pair)
3. strong draws (e.g. Nut flush draw)
4. weak draws and garbage (e.g. gutshot straight-draw)

If you are not the preflop raiser, you should check to the preflop raiser, as they are polarized.

If you are the preflop raiser, then do the following. 
1. On the flop, if your opponent checks to you, bet your strong showdown hands and strongest draws, and check everything else. 
2. If you checked the flop, and your opponent checks the turn, then start betting your medium strength showdown hands and weak draws.

The rough intuition is as follows.

On the flop, you want to be polarized when you bet, betting your strongest hands and some bluffs. The choice of bluffs are your strongest draws because draws benefit from betting, to fold out hands with showdown value, like second pair. Moreover, we want to choose our strongest draws on the flop because we potentially need to bet 3 streets (e.g. with the nut flush draw, we'd want to barrel flop and turn), so we want to use the draws with the most chance of improving. The strongest draws are also capable of calling a raise, which you need to be prepared for if you bet the flop.

If we check flop and are checked to on the turn, then intuitively the opponent's range is generally weaker than it would be had they bet. Against this weaker range, we can start betting our medium strength hands for value, and our weak draws as bluffs, again with the aim of polarizing.

The above strategy is decent as a starting heuristic; better than "I flopped a pair, so I will bet" or "I will bluff beause I feel like it" strategies. However, it certainly is not optimal. As with most games, knowing what's optimal takes a lot of intuition building, through practice and post mortem with a solver or a better player.



