---
layout: post
title:  "Godel's Incompleteness Theorems and Epistemology"
date:   2022-05-16 17:27:18 -0700
categories: jekyll update
---
## Introduction

In the 20th century there was a push to axiomitize math by looking for a set of axioms from which everything useful could be derived. In this search, people considered Peano Arithmetic (PA), a set of 6 intuitive axioms (e.g. reflexivity, transitivity...) plus induction. However, in 1931 Godel released two incompleteness theorems that showed that this was impossible. His first imcompleteness theorem shows the existence of certain statements that are not provable in PA.  Some such statements are of the form "I am not provable". His second incompleteness theorem shows that PA cannot prove its own consistency.

These results are interesting both technically and philosophicallly. Technical-wise, the proof itself uses neat ideas like Chinese Remainder Theorem to encode sequences into integers; further, the result has fascinating connections with computability theory. Philosophhical-wise, people have argued over the interpretation of Godel's theorems since their inception. One canonical interpretation is that Incompleteness shows that the brain is capable of more than a computer (Turing machine). Roger Penrose, the famous mathematical physicist, wrote a book, The Emperor's New Mind, on this argument. Besides cognition and AI, there are also discussions about epistemology ("how do we know things") and the foundations of math.

There's a lot to dig into here, and I hope to cover technical and philosophical sides of the result in this post. The technicals will be based on Steve Cook and Toni Pitassi's lecture notes very closely, while the philosophical side will be based on whatever I find on the internet.

## The First Incompleteness Result

# Proof Sketch

`Notation:` By arithmetic, we mean the vocabulary $$L_A = [0, s, +, \cdot; =]$$. Also, let $$\mathbb{N}$$ be the standard model for $$L_A$$, the usual interpretation of each aforementioned symbol. Finally, let TA (True Arithmetic) be the set of sentences with vocabulary $$L_A$$ that are true under the standard model, $$\mathbb{N}$$. That is, $$TA = \{ \phi : \mathbb{N} \models \phi\}$$

Godel's first incompleteness result shows that arithmetic posses statements that can't be proved. First, we review some concepts from computability theory:

_relation/language_ - a subset of all strings. E.g. Prime(x) is a relation of all strings that encode prime numbers

_r.e._ - a language is recursively enumerable if a Turing Machine (TM) recognizes it (i.e. there's a TM that when given an element in the language, it accepts).

_recursive/computable_ - a language is recursive if a Turing machine decides it (i.e. there's a TM that when given an element in the language, it accepts; when given an element not in the language, it rejects)

Now we are ready to introduce new concepts. 

`Definition:` We say that arithmetic formula A represents relation R if whenever x is in R, A(x) is true in arithmetic, and false otherwise. Formally, suppose R is an n-ary relation, A(x) is a formula such that all free variables in A are $$x_1...x_n$$. Then A(x) represents R iff for all $$x \in \mathbb{N}^n$$,

$$R(a) \iff \mathbb{N} \models A(a)$$

For instance, the relation "x divides y" is represented by the arithmetic formula, 

$$A(x, y) = \exists z (x \cdot z = y)$$

`Definition:` A relation R is arithmetical iff it is represented by some formula.


We now define some special types of arithmetic formulas that we will use to represent relations.

`Definition:` A $$\Delta_0$$ relation is one where the quantified variables are bounded (e.g. $$\forall x \leq 4$$). Further, R(x) is a $$\Delta_0$$ relation iff some $$\Delta_0$$ formula A represents R.

As an example, the relation Prime(x) is represented by the following $$\Delta_0$$ formula.

$$ (s_0 < x) \wedge \forall z \leq x, \forall y\leq x (x = z \cdot y \rightarrow (z = 1 \vee z = x))$$

`Definition:` An $$\exists\Delta_0$$ relation is one of the form $$\exists y A$$, where A is a $$\Delta_0$$ formula. Further, R(x) is a $$\exists\Delta_0$$ relation iff some $$\exists\Delta_0$$ formula A represents R.

Note that $$\Delta_0$$ relations and $$\exists\Delta_0$$ relations are arithmetical. Using these definitions,

`Lemma:` Every $$\Delta_0$$ relation is recursive

This is because we can enumerate over the bounded quantifier of the corresponding arithmetic formula to determine whether a given input is in the relation. $$\blacksquare$$

`Theorem:` Every $$\exists\Delta_0$$ relation is r.e.

An $$\exists\Delta_0$$ relation R is of the form $$\exists y A$$, where A is $$\Delta_0$$ and represents a recursive relation S(x,y). Then, $$R(x) = \exists y R(x,y) $$ and is r.e. $$\blacksquare$$

Note that while every r.e. relation is arithmetical, not every arithmetical relation is r.e. 

Moving forward, it turns out that the converse to the above theorem is also true. This culminates in the famous "Exists-delta" theorem. You can find the argument in reference [1].

`Theorem (Exists Delta):` Every r.e. relation is $$\exists\Delta_0$$

See reference [1] for its elegant proof, which uses Godel's Beta function, a way of encoding sequences of integers (in particular, the cells of the tape of a Turing Machine) into two integers using the Chinese Remainder Theorem.

`Corollary of Exists Delta:` Every relation is r.e. iff it is $$\exists \Delta_0$$. Consider the halting problem, K(x). K(x) is r.e., while $$K^c(x)$$, the complement of the halting problem, is not r.e. Thus, K is represented by some $$\exists \Delta_0$$ formula A(x). Therefore, $$K^c(x)$$ is represented by $$\neg A(x)$$.

$$n \in K^c \iff \neg A(n) \in TA$$

Because $$K^c$$ is not r.e., TA cannot be r.e. either. $$\blacksquare$$

While it doesn't look like it, this last result implies Godel's first incompleteness theorem. To see this, we'll introduce a few more definitions.

Let $$\phi_0$$ denote the set of $$L_A$$ sentences (true or false), that have no free variables. Once again, $$TA = \{ A \in \phi_0 : \mathbb{N} \models A\}$$ stands for True Arithmetic, the set of all true sentences in the language of arithmetic. 

`Definition:` A theory is a set $$\Sigma$$ of sentences closed under logical consequence ($$\models$$). That is, if $$\Sigma \models A$$, then $$A \in \Sigma$$. 

 - $$\Sigma$$ is consistent iff $$\Sigma \neq \phi_0$$. 
 - $$\Sigma$$ is complete iff $$\Sigma$$ is consistent and for all sentences A, $$\Sigma \models A$$ or $$\Sigma \models \neg A$$ (but not both). 
 - $$\Sigma$$ is sound iff $$\Sigma \subset TA$$

 Roughly, a theory is "sound" if it does not have any false statements. A statement is complete if it has all true statements. A theory is consistent if it doesn't have both $$A$$ and $$\neg A$$. Note that if a theory $$\Sigma$$ did have both a statement and its negation, then any statement in $$\phi_0$$ would be a logical consequence of the theory, and thus $$\Sigma = \phi_0$$ (i.e. if we claim that A is both true and false, then anything goes). Note, however, that a theory may have "false" statements but still be consistent, if it doesn't contain the negation.

`Definition:` If $$\Sigma$$ is a theory and $$\Gamma \subset \Sigma$$, then $$\Gamma$$ is a set of axioms for $$\Sigma$$ iff 1) $$\Gamma$$ is recursive and 2) $$\forall A \in \Sigma, \Gamma \models A$$

It turns out the following is true.

`Theorem:` A theory $$\Sigma$$ is axiomitizable iff it is r.e.

We will omit a proof for this, but the immediate consequence of this theorem and the corollary of the Exists-Delta theorem is that TA is not axiomitizable. This is essentially the statement of Godel's First Incompleteness Theorem. Any set of axioms is insufficient for proving all true statements in Arithmetic. No matter which axioms we choose, there is always something outside what we can prove from those axioms that is in TA (i.e. is true under the model $$\mathbb{N}$$).

`Corollary:` TA is not axiomitizable!

Note, however, that TA is complete, sound, and consistent.

`Corollary:` Any sound, axiomitizable theory is incomplete

If $$\Sigma$$ is sound, then $$\Sigma \subset TA$$, and if it is axiomitizable, then $$\Sigma \neq TA$$. Thus, there is an $$A \in TA$$ (that is true) such that $$A \notin \Sigma$$. Furthermore, $$\neg A \notin \Sigma$$ because $$\neg A$$ is false and $$\Sigma$$ is sound. Thus, $$\Sigma$$ is incomplete.


`Remark:` Zermelo Fraenkel set theory with the axiom of choice (ZFC) is regarded as strong enough to formalize all "ordinary" mathematics. If we translated all statements in the language of arithmetic to statements in the language of set theory, there are still sentences in TA whose translations into set theory are not theorems of ZFC.

`Remark:` Although unlikely, there are a number of famous conjectures that you may be true, but do not follow from ZFC.
- Goldbach's conjecture: every even integer is the sum of 2 primes
- Riemann Hypothesis
- P $$\neq$$ NP


# The Arithmetic Hierarchy and Tarski's Theorem

What we showed above is that TA is not r.e. In fact, we can prove a much stronger result: TA is not arithmetical. This is Tarski's Theorem. This result is stronger in that it asserts the complexity of TA is much higher. To make this clear, first we will introduce the Arithmetic Hierarchy (AH), depicted below.

<center><img src="/images/AH.png" alt="drawing" width="350"/></center>

AH is a hierarchy of complexity. Relations that belong to the "higher" sets are more complex in the following sense.

Each set depicted is characterized by arithmetic formulas of a particular form. For instance, $$\Sigma_1$$, on the bottom left, is characterized by $$\exists \Delta_0$$ formulas. As we proved earlier, this set consists of all r.e. relations, because a relation is r.e. iff it is representable by an $$\exists \Delta_0$$ formula. The set $$\Pi_1$$, bottom right, consists of co-RE sets, which are represented by the negation of a $$\Sigma_1$$ formula. For $$k \geq 1$$, we define a $$\Sigma_k$$ formula to be of the form:

$$\exists y_1 \forall y_2 \exists y_3 ... Qy_k A(x, y_1, ... y_k)$$

Where the quantifiers alternate between exists and forall. Note that $$\Sigma_k$$ formulas represent more and more complex relations as k increases. $$\Delta_1$$ contains recursive relations (decidable), while $$\Sigma_1$$ contains r.e. relations that may be undecidable, and $$\Sigma_k$$ contains relations that are far more non-computable.

This hierarchy visualizes what we mentioned before: all r.e. relations are arithmetical (as $$\Sigma_1 \subset AH$$), but not all arithmetical relations are r.e. 

Tarski's theorem says that not only is TA not r.e., it is not even arithmetical! This means that there cannot exist an arithmetic formula that takes as input a formula and is satisfied under $$\mathbb{N}$$ iff that input is in TA. 

TA is not only not-recursive, not-r.e., not-co-r.e.; TA is wildly non-computable! Far harder to compute than any r.e. relation, like the halting problem. In this sense, Tarski's theorem is far stronger than Godel's first incompleteness theorem. 

`Theorem (Tarski):` TA is not arithmetical. More precisely, the relation that takes in a formula and outputs whether it is in TA or not is not arithmetical.

The proof utilizes the "This statement has no proof" construction, and is fun, but mind-bending. Refer to reference [1] for the proof. Finally, it's easy to see that Tarski's theorem implies the result we showed earlier: TA is not r.e.


# Undecidability Results

There are a variety of interesting connections between Incompleteness and Undecidability that we are now in a position to discuss. Roughly speaking, undecidability/decidability corresponds to whether a relation is r.e. or "easier" than r.e. These results, then, discuss the complexity of relations that characterize certain types of theories.

First, we define the following theory, VALID. VALID is the theory (set of theorems) that are true under any interpretation of $$L_A$$ (under any "model"). 

`Definition:` VALID = $$\{ A \in \phi_0 : \models A\}$$

VALID is also the smallest possible theory. For any theory $$\Sigma$$, $$VALID \subset \Sigma$$. It turns out VALID is undecidable, as well

`Theorem (Church):` VALID is undecidable.

We omit the proof.

We also bring up Peano Arithmetic (PA), and RA, which has the first 6 axioms of PA, without the inductive axiom. RA also adds on 3 new axioms to the 6 from PA. To save space, we will not list these axioms here, but most of them are fairly intuitive (reflexivity, symmetry, transitivity, etc.). Finally, we state some theorems about these theories without proof.

Recall from before that sound axiomitizable theories are incomplete. Along somewhat the same lines, 

`Corollary to "RA Representation Theorem":` Every sound extension of RA (including PA) is undecidable. 

An extension of RA is simply a theory that contains RA. So interestingly, any (axiomitizable) theory $$\Sigma$$ such that $$RA \subset \Sigma \subset TA$$ is both undecidable and incomplete.


So far, these have been some undecidability results. Which theories are decidable?

`Theorem (Decidability Theorem):` Every complete, axiomitizable theory is decidable.

From a previous result, it also follows that every complete, axiomitizable theory is not sound. Thus, axiomitizable theories that are complete (i.e. they're consistent and prove either $$A$$ or $$\neg A$$) are not only decidable. However, they necessarily contain some theorems that are not true. 

# Summary

To sum up, Godel's first incompleteness theorem can be thought of as a statement about the complexity or TA (and the inability of a sound, axiomitizable theory to contain all of TA). Using Godel's theorem along with these undecidability results, we depict a visual landscape below that shows VALID, RA and PA, and TA. In this landscape, each theory has two defining parameters: the space it takes up (what theorems it contains) and its complexity.



<center><img src="/images/godel.png" alt="drawing" width="1000"/></center>

_Remark 1:_ Confusingly, Godel also proved "Godel's completeness theorem". However, "completeness" in this theorem is used a different context than "completeness" in his Incompleteness theorems. Completeness in his completeness theorems means completeness in the LK proof-system--every sentence in VALID has an LK proof. In contrast, for a theory to have completeness in his incompleteness theorems, it would have to contain every true sentence in TA. TA is strictly larger than VALID beause VALID are sentences that are true under any model, while TA are sentences that are true under the standard arithmetic model ($$\mathbb{N}$$). Thus, these two theorems are talking about completeness with fundementally different meanings.

_Remark 2:_ The incompleteness and undecidability results arise from the complexity of $$L_A$$. This complexity is derived from there being a $$+$$ and a $$\cdot$$ in the vocabulary. Indeed, if we consider the same vocab but without $$\cdot$$, we get Presburger Arithmetic, which is decidable and has an axiomitization. That is, with just $$+$$, the set of true sentences has much lower complexity than TA. Euclidean Geometry is also a relatively simple theory and is decidable, if I remember correctly. Somehow, TA transcends both these theories in complexity.



## The Second Incompleteness Result

With these strange "This sentence have no proof" constructions in Godel's first theorem, one wonders whether there are actually any meaningful theorems that may not have a proof in PA or ZFC. Godel's second theorem gives a really interesting and meaningful affirmative: 

`Theorem (Godel):` If PA is consistent, then PA cannot prove its own consistency.

## Philosophy

I actually haven't read too much on peoples' arguments here, so I'll just outline the very basics of what I understand people to be arguing. I'll also weave in some of my own thoughts and questions.

# Limits of AI?


# Foundations of Math

# Epistemology


## References

[1] https://www.cs.columbia.edu/~toni/Courses/Logic2021/Notes/Incompleteness.pdf

[2] https://plato.stanford.edu/entries/goedel-incompleteness/#Mat

