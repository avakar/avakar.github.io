---
title: Parameter Estimation by Model Checking for Biological Systems
slug: parameter-estimation-model-checking-biological-systems
tags: [systems biology, sybila, parameter estimation, model checking]
mathjax: true
---

During the last two semesters, I've been working as a member of the laboratory of systems biology on methods for estimation of parameters of biological systems. A paper is currently being written, which will hopefully appear on International Workshop on High Performance Computational Systems Biology (HiBi 2010). Though there still is work to be done, I'd like to summarize the results here in hopes that this text may also serve as a final report for the class "PA183 -- Project in Systems Biology".

## Introduction

The behavior of biological systems can be described in a multitude of ways. One of the common methods -- one that the rest of this article will assume -- is the explicit description by a system of ordinary differential equations.

The state of a system is a vector of substance concentrations $\mathbf x = (x_1, x_2, \dotsc, x_n)$. For each substance there is a differential equation of the form $\dot{x_i} = f_i(\mathbf x)$. Note that we assume piecewise multi-affine systems with ramps only, i.e. $f_i(\mathbf x)$ is a sum of products of atoms, where an atom is either

 * a constant $\kappa$,
 * the concentration of one of the substances $x_j$, or
 * a ramp function $r^+(x_j, \theta^1, \theta^2)$ or $r^-(x_j, \theta^1, \theta^2)$, as described on the [RoVerGeNe homepage][1].

An example of such model would be the following two-gene interaction also taken from the [RoVerGeNe homepage][1].

$$\dot{x_a} = \kappa_a r^-(x_b, \theta^1_b, \theta^2_b)r^-(x_a, \theta^3_a, \theta^4_a) - \gamma_a x_a,$$
$$\dot{x_b} = \kappa_b r^-(x_a, \theta^1_a, \theta^2_a) - \gamma_b x_b.$$

Note, that the system's evolution is deterministic, i.e. given an initial state, there is a single unique curve in the state space of the system that describes system's evolution in time.

Typically, the degradation coefficients $\gamma$ can be easily measured, as they represent the tendency of a substance to dissolve. On the other hand, the kinetic coefficients $\kappa$ are difficult to measure directly.

## Parameter estimation

Instead of trying to measure the parameters directly (which as we noted may be hard), we try to estimate their value by observing the overall system behavior.

There are various numerical methods for determining the best parameter given a run (i.e. a sequence of snapshots of states) of the real system. For example, we could

 1. sample the parameter space,
 2. run a numerical simulation for each sampled parameter value,
 3. compute the distance between the simulated runs and the real run, and
 4. choose the parameter value for which the simulated run is closest to the real one.

Note that the distance between a simulation and a reality is a simple example of a badness function, which can be used as an input to one of the many known global minimization algorithms.

Our approach to parameter estimation is slightly different in that no numerical simulations are performed. Instead, the system's behavior is described by an LTL formula, while the model of the system is abstracted to obtain its finite representation as described by Batt et al.[^1] Our method of parameter estimation, however, fundamentally differs from Batt's.

## Problem reduction

The abstraction as described in the previous paragraph yields a structure that we call a *parametrized [Kripke structure][5]*. It is a graph in which verteces represent sets of states of the original system. There is an edge between two verteces if there is a run in the real system from one of the states of the source vertex to one of the states of the target vertex. (The edge existence can be determined quite efficiently.) Each edge is labeled by the set of parameter values for which it is enabled. The states are labeled by the set of atomic propositions that are satisfied for all states of the vertex.

Note that while all runs of the original system have a corresponding run in the abstraction, there will be additional spurious runs in the abstraction. These spurious runs may reduce the domain of parameters for which a formula is satisfied.

In classical LTL model checking of (non-parametrized) Kripke structures, the LTL formula is converted to a [Buchi automaton][3] the accepting runs of which are exactly those that violate the formula. A synchronous product of the Kripke structure and the aforementioned automaton is computed; fair runs of the resulting fair parametrized Kripke structure are exactly those runs of the original structure that violate the formula.

The original problem is therefore reduced to the problem of determining the set of parameters for which there is no fair run in a fair parametrized Kripke structure.

## Parametrized model checking

We define a *parametrized (fair) Kripe structure* to be a tuple $\mathcal M = (\mathcal P, S, I, F, \to, L)$, where $\mathcal P$ denotes the (possibly infinite) set of parameters, $S$ denotes the finite set of states (or vertices), $I\subseteq S$ the set of initial states, $F\subseteq S$ the set of fair states, $L\colon S\to 2^\Pi$ the labeling of states by atomic propositions (i.e. the interpretation), and $\to\subseteq S\times\mathcal P\times S$ a transition relation labeled by parameters.

A fair $p$-run of $\mathcal M$ is an infinite run of $p$-labeled transitions which passes through a fair state infinitely often. Our goal is to find the maximal set $P\subseteq\mathcal P$ such that there is no $p$-run for any $p\in{P}$. Similarly, we define a $p$-path and a $p$-loop.

If there is to be a fair $p$-run in the structure, there must be a fair state $v$, reachable from an initial state via a $p$-path and there must be a $p$-loop on $v$ consisting of at least one transition. Therefore we compute the result in two nested propagation steps.

A propagation will -- given the set of source vertices -- determine for each vertex the set of parameters $P$ such, that there is a non-trivial $p$-path from a source vertex for all $p\in P$. The final algorithm will then

 1. propagate the parameter domain from initial vertices, computing the set $P_v$ of parameters for which the vertex $v$ is reachable, and
 2. for each fair vertex $v$, propagate $P_v$ from $v$. The parameter set that reaches $v$ is certainly a set of parameters for which there is a fair run.

The algorithm basically performs breath-first passes through the structure and can therefore be easily parallelized.

## Implementation

The above algorithm is implemented -- as a part of the project -- in the [PEPMC][4] tool. The tool expects the specification of the system and of the Buchi automaton on standard input. Examples of these specifications are stored as .bio files in the package.

The syntax of the .bio files is not fixed, but it is in my opinion self-explanatory (given that you've read and understood this article). Following is a sample run of the program.

    $ ./pmc -j 2 < cycle8.bio 
    0/0: 0ms
    1/19683: 8650ms
    2/19683: 0ms
    ((0, 0, 0, 0, 0, 0, 0, 0, 1), 2)<-((0, 1, 0, 0, 0, 0, 0, 0, 1), 2)
    ((0, 1, 0, 0, 0, 0, 0, 0, 1), 2)<-((0, 1, 0, 0, 0, 0, 1, 0, 1), 2)
    ((0, 1, 0, 0, 0, 0, 1, 0, 1), 2)<-((0, 1, 1, 0, 0, 0, 1, 0, 1), 1)
    ((0, 1, 1, 0, 0, 0, 1, 0, 1), 1)<-((0, 0, 1, 0, 0, 0, 1, 0, 1), 1)
    ((0, 0, 1, 0, 0, 0, 1, 0, 1), 1)<-((0, 0, 1, 0, 0, 1, 1, 0, 1), 1)
    ((0, 0, 1, 0, 0, 1, 1, 0, 1), 1)<-((0, 0, 1, 1, 0, 1, 1, 0, 1), 1)
    ((0, 0, 1, 1, 0, 1, 1, 0, 1), 1)<-((0, 0, 0, 1, 0, 1, 1, 0, 1), 1)
    ((0, 0, 0, 1, 0, 1, 1, 0, 1), 1)<-((0, 0, 0, 1, 1, 1, 1, 0, 1), 1)
    ((0, 0, 0, 1, 1, 1, 1, 0, 1), 1)<-((0, 0, 0, 0, 1, 1, 1, 0, 1), 1)
    ((0, 0, 0, 0, 1, 1, 1, 0, 1), 1)<-((0, 0, 0, 0, 0, 1, 1, 0, 1), 1)
    ((0, 0, 0, 0, 0, 1, 1, 0, 1), 1)<-((0, 0, 0, 0, 0, 0, 1, 0, 1), 1)
    ((0, 0, 0, 0, 0, 0, 1, 0, 1), 1)<-((0, 0, 0, 0, 0, 0, 1, 1, 1), 1)
    ((0, 0, 0, 0, 0, 0, 1, 1, 1), 1)<-((0, 0, 0, 0, 0, 0, 0, 1, 1), 1)
    ((0, 0, 0, 0, 0, 0, 0, 1, 1), 1)<-((0, 0, 0, 0, 0, 0, 0, 0, 1), 2)
    [(0.1, 1.1), ((0, 1, 0, 0, 0, 0, 0, 0, 1), 2)]

The `-j 2` parameter specifies that the algorithm should run in two threads. The system in `cycle8.bio` contains 19683 reachable fair states (this is the system after the synchronous product is applied). The initial propagation took 8.7 seconds. The algorithm stopped after propagating from the second fair state, since all parameters were found to be violating. The last line of the output confirms this -- all parameters values in range `[0.1, 1.1]` result in a system violating the formula; the state `((0, 1, 0, 0, 0, 0, 0, 0, 1), 2)` (this is the state number after discretization and synchronous product) is part of the cycle that proves it. The tool outputs the fair cycles as it finds them -- you can use them to debug your system.

## Conclusion

The goals of the project -- develop a tool for parameter estimation -- have been met. However, the Batt's abstraction appears to be very weak. The larger the system is, the more difficult it becomes to ignore the effect of spurious cycles.

Methods of spurious cycle elimination are currently being developed by other members of sybila laboratory. With these methods, PEPMC may in time become of practical use.

  [^1]: G. Batt, C. Belta and R. Weiss. [Model checking genetic regulatory networks with parameter uncertainty][2], 2006, HSCC'07.


  [1]: http://iasi.bu.edu/~batt/rovergene/rovergene.htm
  [2]: http://iasi.bu.edu/~batt/publications/batt_et_al_hscc07.pdf
  [3]: http://en.wikipedia.org/wiki/B%C3%BCchi_automaton
  [4]: http://ratatanek.cz/hg/pepmc
  [5]: http://en.wikipedia.org/wiki/Kripke_structure
