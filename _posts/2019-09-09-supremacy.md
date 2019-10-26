---
layout: post
title:  Quantum supremacy
date:   2019-09-01 16:40:16
description: A comprehensive review of the complexity-theoretical arguments that our current supremacy proposals are based on.
---

<!-- This post is about a particular milestone in the development of quantum computing called *quantum supremacy*. Before discussing it,  -->
Let me briefly set the scene by summarizing the state of quantum computing as a technology: We are currently entering the era of noisy intermediate-scale quantum technology (NISQ) meaning that the qubits making up our devices are prone to error and that we do not have particularly many of them, either. (Current qubit counts are of the order of ~50.)
Realistically, a full-blown large-scale error corrected quantum computer is many, many years if not decades away. Nevertheless, in the next few years, even NISQ devices might already have some useful applications, e.g. in optimization methods. Indeed, identifying more valuable tasks for such devices is a very active area of research. If you are interested in learning more about the NISQ era, I recommend you take a look at the comprehensive and accessible article [Quantum Computing in the NISQ era and beyond](https://arxiv.org/abs/1801.00862) by John Preskill. He goes into more detail in terms of what to expect from near-term devices and potential applications.

In the context of the above, we think of *quantum supremacy* as a precursor to any real application of quantum computing: Rather than doing something useful, it is a term for a proof-of-principle milestone which, once achieved, would rule out once and for all the lurking doubts if quantum computers are really in any sense better than their classical counterparts. Let us thus fix quantum supremacy to have the follwing meaning:

*Quantum supremacy* is the following two-step milestone: 
1. Show rigorously that some task a quantum computer could perform is impossible for a classical computer.
2. Have a quantum device perform the task in an actual experiment and verify the correctness of whatever the output is.

The work on this milestone I want to discuss in this post is of theoretical nature and thus addresses mainly step (1), i.e. the identification of suitable tasks. However, having step (2) in mind, the experimental feasibility of any potential task always guided the focus of research.

But what do we even mean by a rigorous proof of a task being impossible?
A framework that allows one to judge the feasibility of tasks is [Computational complexity theory](https://en.wikipedia.org/wiki/Computational_complexity_theory). It defines the notion of a computational problem and places problems in different complexity classes which, informally, are just sets of problems of similar hardness. The hardness is evaluated on the basis of the time and space resources necessary to solve a problem. A rigorous proof could then consist of showing a *separation* between a class of problems tractable on a quantum computer and class of problems tractable on a classical computer, i.e. by finding a problem that can be shown to belong to one but not the other class. Unfortunately, showing separations in complexity theory is extremely difficult with [P vs. NP](https://en.wikipedia.org/wiki/P_versus_NP_problem) being only the most popular of a plethora of classes suspected but not proved to be unequal. 

The rest of this post makes heavy use of a lot of different complexity classes and definitions are only given for the non-standard ones.

## A chronological perspective

The first paper that I would see as a starting point for proposing or showing quantum supremacy is by [Terhal and DiVincenzo, '02](https://arxiv.org/abs/quant-ph/0205133). They introduce a new model of quantum computation which they call "nonadaptive quantum computation". Back then, the name was clearly inspired by the recent development of [Measurement Based Quantum Computing](https://arxiv.org/abs/quant-ph/0301052) (also know as the One-way model) where measurement outcomes during the computation determine subsequent gates and thus force the computation to adapt at runtime. In contrast, in their model, a computation would never be adapted; instead,<span style="color:red"> one would run the desired computation and measure some qubits only at the end </span>. From the outcomes of these measurements, one could then infer if the expected gates had been applied or not and only accept the computation if all outcomes turned out to right. Take for example...

<img alt="some placeholder" style="max-width:100%" src="some.png"/>

Nowadays, what Terhal and DiVincenzo described in their paper is commonly referred to as *Postselection*: 
<blockquote>
	Postselection is the power of discarding all runs of a computation in which a given event does not occur.
	—Scott Aaronson 
</blockquote>
In the complexity-theoretical sense, postselection is unphysical, because trying again and again is not for free. Nevertheless, it can be used as a tool.
The rather surprising result of their paper was that even constant-depth quantum circuits
Gist of the Terhal paper: First paper to base their results on some complexity-theoretical assumptions, first to use postselection as a method to proof something.


Elucidate to the gap between experiment and complexity theory. (nature communications article)

Asymptotically
this typically requires the precision of each circuit component
must improve by an inverse polynomial in the number of qubits.
This is likely hard to achieve with growing system size without the
use of fault tolerance constructions. More physically reasonable is
to assume that each qubit will at least have a constant error rate,
which corresponds to a total variation distance scaling like O(n).
Recently it was shown that if an IQP circuit has the anti-
concentration property, and it suffers from a constant amount of
depolarizing noise on each qubit then there is an classical
algorithm that can classically simulated it to within a reasonable
total variation distance.49

**This suggests that
unambiguous quantum supremacy may yet require error correc-
tion, though the level of error correction required remains a very
open question.**

**Wie funktioniert der proof via postIQP $\subseteq$ postBQP: aus Douce et al 
 In general, given a restricted model of quantum
computing, if that model becomes universal when supple-
mented with the ability to postselect on a subset of the
outputs, then that model cannot be simulated classically,
otherwise, widely held conjectures of complexity theory
would be violated.