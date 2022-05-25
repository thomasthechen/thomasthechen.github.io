---
layout: post
title:  "Incompleteness, Computability, and Epistemology"
date:   2022-05-16 17:27:18 -0700
categories: jekyll update
---
## Introduction

In the 20th century there was a push to axiomitize math by looking for a set of axioms from which everything useful could be derived. In this search, people considered Peano Arithmetic (PA), a set of 6 intuitive axioms (e.g. reflexivity, transitivity...) plus induction. However, in 1931 Godel released two incompleteness theorems that showed that this was impossible. His first imcompleteness theorem shows the existence of certain statements that are not provable in PA.  Some such statements are of the form "I am not provable". His second incompleteness theorem shows that PA cannot prove its own consistency.

These results are interesting both technically and philosophicallly. From a technical perspective, the result has fascinating connections with computability theory; also, the proof itself uses neat ideas like Chinese Remainder Theorem to encode sequences into integers. Philosophically, people have argued over the interpretation of Godel's theorems since their inception. One canonical interpretation is that Incompleteness shows that the brain is capable of more than a computer (Turing machine). Roger Penrose, the famous mathematical physicist, wrote a book, The Emperor's New Mind, on this argument. Besides cognition and AI, there are also discussions about epistemology ("how do we know things") and the foundations of math.

There's a lot to dig into here, and I hope to cover technical and philosophical sides of the result in this post. The technicals will be based on Steve Cook and Toni Pitassi's lecture notes very closely, while the philosophical side will be based on whatever I find on the internet.

## The First Incompleteness Result

# Proof Sketch

`Notation:` By arithmetic, we mean the sentences formed from the vocabulary $$L_A = [0, s, +, \cdot; =]$$. Also, let $$\mathbb{N}$$ be the standard model for $$L_A$$: which assigns each symbol their usual interpretation (e.g. $$\cdot$$ means multiply). Finally, let TA (True Arithmetic) be the set of sentences with vocabulary $$L_A$$ that are true under the standard model, $$\mathbb{N}$$. That is, $$TA = \{ \phi : \mathbb{N} \models \phi\}$$, where we use $$\models$$ to denote "logical consequence". In particular, $$\mathbb{N} \models \phi$$ means that $$\phi$$ is a logical consequence of $$\mathbb{N}$$, the standard interpretation of arithmetic.

Godel's first incompleteness result shows that TA cannot be "axiomitized". For any (recursive) set of axioms, such as PA, there is always some statement in TA that can't be proved from them. First, we review some concepts from computability theory:

_relation/language_ - a relation (equivalently, a language) is a subset of all strings. E.g. Prime(x) is a relation of all strings that encode prime numbers

_r.e./recognizable_ - a language is recursively enumerable (equivalently, recognizable) if a Turing Machine (TM) recognizes it (i.e. there's a TM that when given an element in the language, it accepts).

_recursive/computable/decidable_ - a language is recursive (equivalently, computable or decidable) if a Turing machine decides it (i.e. there's a TM that when given an element in the language, it accepts; when given an element not in the language, it rejects)

Now we are ready to introduce new concepts. 

`Definition:` We say that arithmetic formula A represents relation R if whenever x is in R, A(x) is true in arithmetic, and false otherwise. Formally, suppose R is an n-ary relation, A(x) is a formula such that all free variables in A are $$x_1...x_n$$. Then A(x) represents R iff for all $$x \in \mathbb{N}^n$$,

$$R(a) \iff \mathbb{N} \models A(a)$$

For instance, the relation "x divides y" is represented by the arithmetic formula, 

$$A(x, y) = \exists z (x \cdot z = y)$$

`Definition:` A relation R is arithmetical iff it is represented by some formula.


We now define some special types of arithmetic formulas that we will use to represent relations.

`Definition:` A $$\Delta_0$$ relation is one where the quantified variables are bounded (e.g. $$\forall x \leq 4$$). Further, R(x) is a $$\Delta_0$$ relation iff some $$\Delta_0$$ formula A represents R.

As an example, the relation Prime(x) is represented by the following $$\Delta_0$$ formula, which roughly says that "x is bigger than 1 (aka the successor of 0, s0), and for any y less than x that divides x, the remainder is either 1 or x".

$$ (s0 < x) \wedge \forall z \leq x, \forall y\leq x (x = z \cdot y \supset (z = 1 \vee z = x))$$

`Definition:` An $$\exists\Delta_0$$ relation is one of the form $$\exists y A$$, where A is a $$\Delta_0$$ formula. Further, R(x) is a $$\exists\Delta_0$$ relation iff some $$\exists\Delta_0$$ formula A represents R.

Note that $$\Delta_0$$ relations and $$\exists\Delta_0$$ relations are arithmetical. Using these definitions, we can make two claims.

`Lemma:` Every $$\Delta_0$$ relation is recursive

_Proof:_ This is because we can enumerate over the bounded quantifier of the corresponding arithmetic formula to determine whether a given input is in the relation. $$\blacksquare$$ 

`Theorem:` Every $$\exists\Delta_0$$ relation is r.e.

_Proof:_ An $$\exists\Delta_0$$ relation R is of the form $$\exists y A$$, where A is $$\Delta_0$$ and represents a recursive relation S(x,y). Then, $$R(x) = \exists y R(x,y) $$ and is r.e. $$\blacksquare$$

Regarding the first lemma, note that the converse is not true: not every recursive relation is $$\Delta_0$$. Regarding the second theorem, note that while every r.e. relation is arithmetical, not every arithmetical relation is r.e. 

Moving forward, it turns out that the converse to the above theorem is also true. This culminates in the famous "Exists-Delta" theorem.

`Theorem (Exists Delta):` Every r.e. relation is $$\exists\Delta_0$$

See reference [1] for its elegant proof. The proof hinges on the fact that the "graph" of a recursive function is $$\exists\Delta_0$$. To show this, you use Godel's Beta function, a way of encoding sequences of integers (in particular, the cells of the tape of a Turing Machine) into two integers using the Chinese Remainder Theorem.

`Corollary of Exists Delta:` TA is not r.e.

_Proof:_ By the two previous theorems, every relation is r.e. iff it is $$\exists \Delta_0$$. Consider the halting problem, K: on input x, K(x) = 1 if x encodes a TM that halts, and is 0 otherwise. It is well known that K is r.e., while $$K^c$$, the complement of the halting problem, is not r.e. Thus, K is represented by some $$\exists \Delta_0$$ formula A(x), which crucially means that $$K^c(x)$$ is represented by $$\neg A(x)$$.

$$n \in K^c \iff \neg A(n) \in TA$$

This "reduction" argument means that if TA is r.e., then $$K^c$$ is also r.e ("we can list off elements in $$K^c$$ if we are able to list off elements in TA"). Thus, because $$K^c$$ is not r.e., TA cannot be r.e. either. $$\blacksquare$$

This last corollary is a statement about the complexity of TA. While it doesn't look like it, it implies Godel's first incompleteness theorem. To see this, we'll introduce a few more definitions.

Let $$\phi_0$$ denote the set of $$L_A$$ sentences (true or false), that have no free variables. Once again, $$TA = \{ A \in \phi_0 : \mathbb{N} \models A\}$$ stands for True Arithmetic, the set of all true sentences in the language of arithmetic. 

`Definition:` A theory is a set $$\Sigma$$ of sentences closed under logical consequence ($$\models$$). That is, if $$\Sigma \models A$$, then $$A \in \Sigma$$. 

 - $$\Sigma$$ is consistent iff $$\Sigma \neq \phi_0$$. 
 - $$\Sigma$$ is complete iff $$\Sigma$$ is consistent and for all sentences A, $$\Sigma \models A$$ or $$\Sigma \models \neg A$$ (but not both). 
 - $$\Sigma$$ is sound iff $$\Sigma \subset TA$$

 Roughly, a theory is sound if it does not have any false statements. A statement is complete if it contains all possible true statements. A theory is consistent if it doesn't have both $$A$$ and $$\neg A$$. Note that if a theory $$\Sigma$$ did have both a statement and its negation, then any statement in $$\phi_0$$ would be a logical consequence of the theory, and thus $$\Sigma = \phi_0$$ (i.e. if we claim that A is both true and false, then anything goes). Note, however, that a theory may have "false" statements but still be consistent, if it doesn't contain the corresponding negation.

`Definition:` If $$\Sigma$$ is a theory and $$\Gamma \subset \Sigma$$, then $$\Gamma$$ is a set of axioms for $$\Sigma$$ iff 1) $$\Gamma$$ is recursive and 2) $$\forall A \in \Sigma, \Gamma \models A$$

It turns out the following is true.

`Theorem:` A theory $$\Sigma$$ is axiomitizable iff it is r.e.

We will omit a proof for this, but the immediate consequence of this theorem and the corollary of the Exists-Delta theorem is that TA is not axiomitizable. This is essentially the statement of Godel's First Incompleteness Theorem. Any set of axioms is insufficient for proving all true statements in True Arithmetic. No matter which axioms we choose, there is always something in TA that's outside what we can prove from those axioms.

`Theorem (Godel):` TA is not axiomitizable!

Although not axiomitizable, note that TA is complete, sound, and consistent. Thus,

`Corollary:` Any sound, axiomitizable theory is incomplete

_Proof:_ If $$\Sigma$$ is sound, then $$\Sigma \subset TA$$, and if it is axiomitizable, then $$\Sigma \neq TA$$. Thus, there is an $$A \in TA$$ (that is true) such that $$A \notin \Sigma$$. Furthermore, $$\neg A \notin \Sigma$$ because $$\neg A$$ is false and $$\Sigma$$ is sound. Thus, $$\Sigma$$ is incomplete. $$\blacksquare$$


`Remark:` Zermelo Fraenkel set theory with the axiom of choice (ZFC) is regarded as strong enough to formalize all "ordinary" mathematics. If we translated all statements in the language of arithmetic to statements in the language of set theory, there are still sentences in TA whose translations into set theory are not theorems of ZFC.

`Remark:` Although unlikely, it's possible that a number of famous conjectures that may be true, but do not follow from ZFC.
- Goldbach's conjecture: every even integer is the sum of 2 primes
- Riemann Hypothesis
- P $$\neq$$ NP


# The Arithmetic Hierarchy and Tarski's Theorem

What we showed above is that TA is not r.e. In fact, we can prove a much stronger result: TA is not arithmetical. This is Tarski's Theorem. This result is stronger in that it asserts the complexity of TA is much higher than simply "not r.e.". To make this clear, first we will introduce the Arithmetic Hierarchy (AH), depicted below.

<center><img src="/images/AH.png" alt="drawing" width="350"/></center>

AH is a hierarchy of relations (languages) by complexity. Relations that belong to the "higher" sets are more complex in the following sense.

Each set that's depicted is characterized by arithmetic formulas of a particular form. For instance, $$\Sigma_1$$, on the bottom left, is characterized by $$\exists \Delta_0$$ formulas. As we proved earlier, this set consists of all r.e. relations, because a relation is r.e. iff it is representable by an $$\exists \Delta_0$$ formula. The set $$\Pi_1$$, bottom right, consists of co-RE sets, which are represented by the negation of a $$\Sigma_1$$ formula. For $$k \geq 1$$, we define a $$\Sigma_k$$ formula to be of the form:

$$\exists y_1 \forall y_2 \exists y_3 ... Qy_k A(x, y_1, ... y_k)$$

Where the quantifiers alternate between exists and for-all. Note that $$\Sigma_k$$ formulas represent more and more complex relations as k increases. You're probably familiar with $$\Delta_1$$, which contains recursive relations (decidable by a TM), and $$\Sigma_1$$, which contains r.e. relations (recognizable by a TM). But there are more complex relations, such as $$\Sigma_k$$, that are far more non-computable (No TM can even hope to recognize such a language).

This hierarchy visualizes what we mentioned before: all r.e. relations are arithmetical (as $$\Sigma_1 \subset AH$$), but not all arithmetical relations are r.e. 

Tarski's theorem says that not only is TA not r.e., it is not even arithmetical. It isn't co-r.e., $$\Sigma_2$$, $$\Sigma_3,$$ etc. TA is wildly non-computable-- far harder to compute than something like the Halting Problem or its complement! In this sense, Tarski's theorem makes a far stronger statement about TA's complexity than Godel's first incompleteness theorem. 

`Theorem (Tarski):` TA is not arithmetical.

_Proof:_ The proof is based on the following idea using self-referential sentences. Consider the sentence "I am false", call it A. If A is true, then this means that A is false, which is a contradiction. By itself, this isn't an actual argument and (formally) isn't a contradiction.

However, now suppose (towards contradiction) that TA is arithmetical. This means that there exists an arithmetical formula, R, that when given a statement in arithmetic, tells you whether that statement is true (i.e. in TA). This is a strong assumption since it implies that whether an arithmetic statement is true can be succinctly captured in some formula.

Then, we reach a contradiction if we ask what R outputs on A. If R outputs "true" on A, then A must be false--If R outputs "false" on A, A must be true!

$$R(A) = 1 \iff A \text{ is false} \iff R(A) = 0$$

We conclude that such a formula, R, cannot exist. That is, TA is not arithmetical. On a high level, determining whether an arithmetic statement is true is an exceedingly complicated function, and cannot be captured by any particular arithmetic formula (in the way that, say, Prime(x) can). TA is too "complex" to be captured by AH. $$\blacksquare$$

The real proof in reference [1] is essentially a formal way of writing what we sketched above. Finally, since $$\Sigma_1 \subset AH$$, Tarski's theorem immediately implies the result we showed earlier: TA is not r.e.


# Undecidability Results

There are a variety of interesting connections between Incompleteness and Undecidability that we are now in a position to discuss. Roughly speaking, undecidability/decidability corresponds to whether a relation is r.e. or "easier" than r.e. These results are about the complexity of a theory (a set of theorems, closed under logical consequence), in relation to certain properties of the theory (soundness, completeness, etc.)

First, we define the theory, VALID. VALID is the theory that are true under any interpretation of $$L_A$$ (under any "model"). 

`Definition:` VALID = $$\{ A \in \phi_0 : \models A\}$$

VALID is also the smallest possible theory. For any theory $$\Sigma$$, $$VALID \subset \Sigma$$. It turns out VALID is undecidable, as well

`Theorem (Church):` VALID is undecidable.

We omit the proof.

We also bring up Peano Arithmetic (PA), and RA, which has the first 6 axioms of PA, without the inductive axiom. RA also adds on 3 new axioms to the 6 from PA. To save space, we will not list these axioms here, but most of them are fairly intuitive (reflexivity, symmetry, transitivity, etc.). Finally, we state some theorems about these theories without proof.

Recall from before that sound axiomitizable theories are incomplete. Along similar lines, 

`Corollary of RA Representation Theorem:` Every sound extension of RA (including PA) is undecidable. 

An extension of RA is simply a theory that contains RA. So interestingly, any (axiomitizable) theory $$\Sigma$$ such that $$RA \subset \Sigma \subset TA$$ is both undecidable and incomplete.


So far, these have been some undecidability results. Which theories are decidable?

`Theorem (Decidability Theorem):` Every complete, axiomitizable theory is decidable.

From a previous result, it also follows that every complete, axiomitizable theory is not sound. Thus, axiomitizable theories that are complete (i.e. they're consistent and prove either $$A$$ or $$\neg A$$) are not only decidable, they are necessarily unsound (containing untrue theorems).

# Final Remarks

To sum up, Godel's first incompleteness theorem can be thought of as a statement about the complexity of TA (and the inability of a sound, axiomitizable theory to encompass all of TA). Using Godel's theorem along with these undecidability results, we depict a visual landscape below that shows VALID, RA and PA, and TA. In this landscape, each theory has two defining parameters: the space it takes up (what theorems it contains) and its complexity.



<center><img src="/images/godel.png" alt="drawing" width="1000"/></center>

_Remark 1:_ Confusingly, Godel also proved "Godel's completeness theorem". However, "completeness" in that theorem is used a different context and harbors a different meaning. Completeness in his completeness theorems refers to completeness of the LK proof-system: every sentence in VALID has an LK proof. In contrast, for a theory to have completeness in his incompleteness theorems, it would have to contain every true sentence in TA. TA is strictly larger than VALID beause VALID are sentences that are true under any model, while TA are sentences that are true under the standard arithmetic model ($$\mathbb{N}$$). Thus, these two theorems are talking about completeness with fundementally different meanings, and do not contradict.

_Remark 2:_ The incompleteness and undecidability results arise from the complexity of $$L_A$$. This complexity is derived from there being a $$+$$ and a $$\cdot$$ in the vocabulary. Indeed, if we consider the same vocabulary but without $$\cdot$$, we get Presburger Arithmetic, which is decidable and has an axiomitization. That is, with just $$+$$, the set of true sentences has much lower complexity than TA. Euclidean Geometry is also a relatively simple theory and is decidable, if I remember correctly. Somehow, TA transcends both these theories in complexity.



## The Second Incompleteness Result

In its original form, Godel's first incompleteness theorem showed the existence of theorems with no proof in PA. Our discussion above applies more generally to any sound, axiomitizable theory, but let's reduce our scope a little and look at Godel's original construction.

His original construction was similar to the one in the proof of Tarski's Theorem: there's a sentence G, encoded in arithmetic, that roughly says "I have no proof." This sentence is true (as if it were false, it would have a proof, violating soundness of PA!), yet it is not a consequence of PA. 

`Theorem (Godel's First Incompleteness Theorem, Original form):` If PA is consistent, then PA does not prove G.

With these strange self-referential statements underpinning Godel's first theorem, one wonders whether there are actually any meaningful theorems that may not have a proof in PA or ZFC. Godel's second theorem offers a more satisfying "unprovable" theorem. 

`Theorem (Godel's Second Incompleteness Theorem):` If PA is consistent, then PA cannot prove its own consistency.

Refer to [1] for the proof, but roughly, one can show that PA proves the statement "If PA were consistent, then G is true". Thus, by the first incompleteness theorem, if PA is consistent, then PA cannot prove its own consistency, or else it would also prove G. 

Note that since G is "I am not provable in PA", then the statement "If PA were consistent, then G is true" is equivalent to "If PA were consistent, then G is not provable in PA." This is exactly the statement of the first incompleteness theorem. Thus, the crux of the proof for the second incompleteness theorem is to show that the first incompleteness theorem can be formalized in PA.

## Philosophy

As if this all wasn't mind-bending enough, now we'll get into higher level, philosophical interpretations of Godel's theorems. I actually haven't read these in too much depth, so I'll just outline most basic arguments, drawing ideas from Roger Penrose, Scott Aaronson, Alan Turing, and the depths of the credible internet. I'll also weave in some of my own thoughts and questions.

# Limits of AI?



# Foundations of Math

# Epistemology


## References

[1] https://www.cs.columbia.edu/~toni/Courses/Logic2021/Notes/Incompleteness.pdf

[2] https://plato.stanford.edu/entries/goedel-incompleteness/#Mat

