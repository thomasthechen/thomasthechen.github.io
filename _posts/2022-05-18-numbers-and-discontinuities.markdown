---
layout: post
title:  "The Discontinuity Between 2 and 3"
date:   2022-05-17 17:27:18 -0700
categories: jekyll update
---
## Introduction

"Two's company, three's a crowd" captures what I think is a deep insight in the field of Computational Complexity, which studies the computational resources needed to solve different computational tasks. A computational task is any problem you can use a computer to solve. The task can be "easy" in that there exists an algorithm that runs in polynomial time (e.g. Given an integer n, compute the nth fibonacci number), or "hard" in that any algorithm solving the task requires exponential time to solve (e.g. Given a graph G, determine if there is a cycle that visits each vertex exactly once). In addition, complexity theory groups problems into classes such as P or NP. Roughly, we say a problem is hard relative to a class (e.g. 3-SAT is NP-hard) if it is at least as difficult, computationally, as any problem in the class. We say a problem is complete for a class if it is hard relative to that class, but also in that class (e.g. 3-SAT is NP-complete since it is NP-hard, but also is a problem in NP).


## A Jump in Complexity

What's interesting is that certain computational tasks are "easy" when they involve two things, but "hard" when there are three or more things. The cleanest example of this is the dichotomy between 2-SAT and 3-SAT.

A boolean function takes in boolean variables $$x_1...x_n \in \{ 0, 1\}$$ and outputs 0 or 1. A k-CNF formula is a special type of boolean function that is an "'and' of 'or's". As an example, formula (1) is a 2-CNF formula while formula (2) is a 3-CNF formula. The difference bewteen them is that each inner "clause" (which are shown surrounded by parentheses) consists of 2 literals in the 2-CNF formula, and 3 literals in the 3-CNF formula (a literal is a variable $$x_i$$ or its negation $$\neg x_i$$).

$$\phi_2 = (x_1 \vee x_2) \wedge (\neg x_3 \vee x_4) \wedge (x_1 \vee x_6)$$

$$\phi_3 = (x_1 \vee x_2 \vee x_4) \wedge (\neg x_2 \vee x_3 \vee x_4) \wedge (x_1 \vee x_5 \vee x_6)$$

The dichotomy between 2-SAT and 3-SAT is as follows. Supppose we are given a 2-CNF formula $$\phi_2$$ and a 3-CNF formula $$\phi_3$$, both on n variables. Because $$\phi_2$$ is in 2-CNF form, we can actually determine, in a small amount of time, whether there exists a setting of 0's and 1's to variables $$x_1, ... x_n$$ such that $$\phi_2$$ evaluates to 1. That is, we can determine if $$\phi_2$$ has a _satisfying_ assignment in polynomial time (here, polynomial time is considered "small", in contrast to exponential time, which is "large"). This computational task, where we want to decide if a given 2-CNF formula has a satisfying assignment, is called 2-SAT.

In contrast, because $$\phi_3$$ is a 3-CNF formula, we strongly believe that there is no "fast" algorithm for determining whether it has a satisfying assignment. Any algorithm to find such an assignment takes exponential time, not polynomial time. This computational task is called 3-SAT.

Thus, there is a gap in difficulty between 2-SAT and 3-SAT. This is interesting since these tasks are analogous (both involve finding satisfying assignments of CNF's); they differ only in a certain parameter, k, for the size of clauses. What's more, the problem of finding a satisfying assignment for a k-CNF formula, $$k \geq 4$$ are all "equivalent" in complexity to finding a satisfying assignment of some $$3-CNF$$. That is, if we are able to solve 3-SAT, we can solve k-SAT for any $$k$$: $$k = 4, 5, 6, ...$$. In this sense, $$3-SAT$$ is just as hard as $$k-SAT$$, $$ k \geq 4$$. Thus, there is a jump in complexity between 2-SAT and 3-SAT, but after that, as k increases, the complexity of k-SAT saturates, as the complexity of 4-SAT, 5-SAT, 6-SAT are all the same as that of 3-SAT. Somehow, 2 and 3 have this special "jump" in complexity.


There seem to many other such phenomena in Complexity: in game theory, we can study the complexity of computing a Nash Equilibrium (NE)--how many resources it takes to seaerch for a NE. Computing a NE of two players, 2-Nash, is PPAD-complete (Chen-Deng, 2006), while computing a NE of three players, 3-Nash, is FIXP-complete (Etessami-Yannakakis, 2007). Here, PPAD and FIXP are complexity classes, where FIXP contains PPAD (Thus, a problem that is FIXP-hard is also PPAD-hard). Assuming $$PPAD \neq FIXP$$, this indicates that 3-Nash is harder than 2-Nash, a jump in complexity similar to that of 2-SAT and 3-SAT.

So what? Of course adding more players increases the complexity of finding a NE! But here's the kicker: every n-player game is in a sense "equivalent" to a 3-player game, in that there is a polynomial time transformation that recovers a NE of any n-player game from that of some 3-player game (i.e. $$n-Nash \leq 3-Nash$$, Bubelis, 1979).

So there is a gap in complexity between 2-Nash and 3-Nash, but after 3-Nash, the complexity saturates as the number of players increases. The only discontinuity in complexity is in the jump from 2 and 3 players. That's really interesting, and it's the same phenomena we saw with 2-SAT/3-SAT. 

What's special about 2 and 3?

## Other Interesting Examples

Before attempting to answer that question, I want to argue that this phenomena isn't something confined to the monastic theory of complexity. Here are some other more tangible problems where there is a "jump" between 2 and 3.

_Three body problem_ - When we have isolated two bodies interacting through gravitation, they follow a stable elliptical orbit around their center of mass (E.g. 2 stars in a "binary star"). But as soon as we introduce a third body, the orbit becomes chaotic and irregular. We no longer have a nice elliptical orbit, but have something unpredictable, sensitive to initial conditions, and analytically unsolvable, apparently. 

_Random Walks in 3 dimensions_  - Suppose that we have a particle at the origin of a 3D grid. At each time step, the particle will move one unit left, or one unit right with equal probability, 1/2. This is called a random walk in 1 dimension. It turns out that in 1 dimension, the particle will return to the origin infinitely often. So if I start the random walk then go play a game of league, when I come back, I know that I will be able to see the particle return to the origin after waiting some finite amount of time. 

Suppose that the particle now does a 2D random walk: at each time step, it goes left, right, forward, or backwards with equal probability 1/4. As with the 1D case, the particle still returns to the origin infinitely often. However, when the particle does a 3D random walk, moving either left, right, forward, backwards, up, or down with equal probability, then it no longer returns to the origin infinitely often. Somehow, as soon as we move to 3 dimensions, there is a tendency for the particle to stray away ("diffuse") from the origin and never come back.

_Fermat's Last Theorem_ - We can find integers $$a,b,c$$ such that $$a^2 + b^2 = c^2$$, e.g. $$a = 3, b = 4, c = 5$$. But for any integer power $$n \geq 3$$, there is no integer solution to $$a^n+ b^n = c^n$$. 

Because these problems aren't rigorously connected to a notion of "complexity", I won't claim that the phenomena we see here is the same as we saw with the jump in complexity in the previous section. However, what seems to be true is that there is some fundemental jump between parameters 2 and 3 in how these problem play out. 3 seems to be the minimal parameter where the problem's behavior changes.

## Why 2 and 3

The examples we gave above were interesting, but I only have concrete things to say about the earliest example--2-SAT and 3-SAT. We'll examine a cornerstone result of Complexity and NP-completeness: the Cook-Levin Theorem. If you want to skip the technical portion, go to the paragraph labeled __(**)__. However, I'll do my best to include only necessary details and keep the rest high level.

NP is a complexity class that contains problems whose solution can be "verified" in a small amount of time. That is, suppose that you are given a 3CNF formula. It might be hard for you to decide if a satisfying assignment exists for it (maybe taking exponential time). However, if I were to hand you an assignment of the variables (e.g. $$x_1 = 1, x_2 = 0, x_3 = 1, ...x_n = 1$$), then you can easily plug in those values for the variables and check that what I gave you is indeed a satisfying assignment. This is what we mean when we say a problem's solution can be verified easily; indeed, 3-SAT is in NP. For a lot of problems in NP, there may not be fast algorithms to find a solution, but there have to be fast algorithms to verify a solution's correctness.


The Cook-Levin Theorem states that 3-SAT is NP-hard. Computationally, 3-SAT is at least as hard as any problem in NP. The proof is insightful into why 3CNF's capture the complexity of all of NP, while 2CNF's cannot.  

Suppose we're given a language $$L \in NP$$, with a verfiier M. This means that a problem instance x is in L iff there is solution $$u$$ to problem instance x. If this is the case, then $$M(x, u) = 1$$: that is, M verifies that u is a solution to x. Otherwise, if u is not a solution to x, then $$M(x,u) = 0$$.

Now, we'll sketch the proof of the Cook-Levin Theorem, following ch 6 of Arora's textbook.

`Claim 1:` For any polynomial time turing machine M, we can create a family of polynomial sized circuits that is equivalent to M (i.e. they compute the same function).

_Proof:_ Omitting technical definitions, this essentially follows from the fact that $$P \subset P/poly$$. The takeaway of this claim is that our verifier, M, can be expressed as a set of circuits. In particular, if we fix the input size of M, then we can express M as one circuit C. If you've taken systems, this claim is pretty intuitive as any computational machine is comprised of a circuit of AND, OR, and NOT gates. $$\blacksquare$$

Define the language CKT-SAT to have all boolean circuits (outputting 0 or 1) that have a satisfying assignment: a circuit C is satisfiable if there exists a $$u \in \{ 0, 1\}^n$$ such that C(u) = 1. 

`Claim 2:` CKT-SAT is NP-hard.

_Proof:_ For $$L \in NP$$, let M be its verifier, and for any instance $$x \in L$$, encode $$M(x, \cdot)$$ as a circuit $$C(\cdot)$$ such that $$M(x, u) = C(u), \forall u$$. Then, if there is a solution u to x, then $$M(x, u) = 1 = C(u)$$. That is, u satisfies C. Similarly, if there is no such solution u, then C is unsatisfiable.

Thus, $$x \in L \iff C \in CKT-SAT$$. This means that if we want to decide if a problem instance x is in an NP language L, we can instead decide whether a particular circuit C is in CKT-SAT, solving the latter as a "proxy," of sorts. Thus, if we have the power to decide CKT-SAT, by extension we are able to decide any language in NP. That is, deciding whether a circuit $$C \in CKT-SAT$$ is at least as hard as deciding any language in NP. $$\blacksquare $$

So far, we've "encoded" NP computations into deciding whether a particular circuit is satisfiable, an NP-hard problem. This circuit will only output 1 for certain inputs but not others, and this can be thought of as the circuit imposing a set of constraints on the input. Only inputs that satisfy the constraints will cause the circuit to output 1. Inputs that don't satisfy the constraints will cause the circuit to output 0. The set of constraints induced by the circuit form a constraint satisfaction problem (CSP). 3-SAT is a type of CSP, where each constraint is a "clause" like we discussed before. What's interesting is that despite 3-SAT's constraints taking a relatively simple form, they are sufficient for encoding general constraints imposed by a circuit. In particular,

`Claim 3:` $$CKT-SAT \leq 3-SAT$$

_Proof:_ We encode C as a set of constraints as follows. For each node $$v_i$$ of C, we have a corresponding variable $$z_i$$ in our formula $$\phi$$. If node $$v_i$$ is an AND of nodes $$v_j, v_k$$, then we add the follow constraint to $$\phi$$.

$$z_i = (z_j \wedge z_k)$$

This can be written in 3-CNF form as

$$(\neg z_i \vee \neg z_j \vee z_k) \wedge (\neg z_i \vee z_j \vee \neg z_k) \wedge (\neg z_i \vee z_j \vee z_k) \wedge (z_i \vee \neg z_j \vee \neg z_k)$$

We can encode OR, NOT gates similarly. Finally, if $$v_i$$ is the output node of C, then we add the clause $$(z_i)$$ to $$\phi$$. Then, $$\phi$$ is satisfiable iff C is satisfiable. This construction implies that being able to decide whether 3-CNF formulas are satisfiable is at least as hard as deciding circuit satisfiability. Thus, 3-SAT is NP-hard. $$\blacksquare$$

The arguments here "encodes" each computation in NP first as a circuit, then as a set of 3-SAT constraints. The fact that we can always encode in this way is the reason for hardness. 

__(**)__ Returning to our question, notice that a constraint of size 3 is the smallest size constraint we can use to encode NP computations. The basic building block of a circuit are AND, OR, and NOT gates, and we, at least, need to be able to encode these as constraints to encode an arbitary circuit (and by extension, an NP-computation). These gates impose constraints over 3 variables: the two inputs, and the output. Thus, only a CSP with constraints of size at least 3 can encode arbitrary circuits. 

This is why 3-SAT can encode NP-computations while 2-SAT cannot. Indeed, if we could only have constraints over two variables, then we wouldn't be able to encode the constraints that the circuit imposes on its input. Size-2 constraints aren't sufficiently "complex" to capture whether a circuit is satisfiable; whether an NP instance has a solution. If we think in terms of circuit components, with two variables, we can't encode AND or OR gates; we can only encode NOT and Identity gates.


_remark_: What we showed above is not the canonical proof for the Cook-Levin Theorem, but the canonical proof illustrates the same ideas, though it takes longer to explain. On a high level, that proof encodes the tableaux of the verifier M as a 3-CNF formula. The tableaux is the "history of computation" of M, which tracks the configurations/states it was in at each timestep until it finished its computation. Intuitively, a CSP can be created to be satisfied given a valid tableaux, that was generated by a Turing machine. Because turing machines operate on a small number of inputs, these constraints are highly local and therefore encodable in a small number of variables each. In short, the nature of a Turing machine is that it only acts on a small number of bits at a time, enabling us to create a 3-CNF formula encoding whether it outputted 1 (i.e. verified a solution to an instance x in L).

## Concluding Remarks

In wondering why 2-SAT does not capture the complexity of NP while 3-SAT does, we've provided a rough answer: circuits (or other models of computation) can only be described by constraints of size at least 3. These are exemplified by AND and OR gates, which at minimum have 2 inputs and one output. I guess the natural follow up is: how is it that any circuit computing any function can be expressed as a combination of AND, OR, and NOT gates? (and beyond classical computing, an analogous result also holds for quantum computing--see the Solovay-Kitaev Theorem). For this, I don't have any technicals to present. But it is really interesting that arbitrary, complex computations are comprised of these small components that do simple logical operations. Each component follows simple rules, but the combination of them makes the complexity of the whole very large--like that scene out of 三体.

<!--

# Cook Levin Theorem alternate proof

The high level idea of the Cook-Levin Theorem is that the Verifier, M,'s computation is encodable as a set of constraints, in a 3-CNF formula. The 3 CNF formula, being an "and" of clauses, can be thought of as a set of constraints on the variables $$x_1, ... x_n$$, where each constraint corresponds to one clause in the 3-CNF. In particular, we can encode the function $$M(x, \cdot)$$ as a boolean function in 3CNF form. 


## Appendix

-->





