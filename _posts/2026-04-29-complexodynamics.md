---
title: 'Notes: The First Law of Complexodynamics'
date: 2026-04-29
permalink: /posts/2026/04/notes-complexodynamics/
tags:
  - ilya-30
---

Original: https://scottaaronson.blog/?p=762

## Why does "complexity" or "interestingness" of physical systems seem to increase with time and then hit a maximum and decrease in contrast to entropy, which of course increases monotonically?

Entropy: referring to the physics term, entropy always increasing until heat death of the universe

Complexity/Interestingness: how much structure a system has

Start of big bang (low entropy, low complexity) -> galaxies/stars (mid entropy, high complexity) -> heat death of the universe (high entropy, low complexity)

Essentially, **why is entropy strictly increasing while complexity is a bell curve?**

---

Second Law of Thermodynamics: entropy of any closed system increases with time until it reaches a maximum value

- So how did the universe have low entropy to begin with? Unanswered

How exactly do we define complexity?

- Clearly at a low entropy state, complexity is close to zero. Entropy plausibly gives an upper bound on complexity.
- At a higher state of entropy, the system reaches equilibrium, which we define as simple.

## How to define entropy?

Kolmogorov complexity: the length of a shortest computer program that produces the object as output, measure of computational resources needed to specify an object

Assume we can use Kolmogorov complexity to define entropy (why idk):

- If the system is deterministic:
  - State can be represented by initial state (constant bits) and number of steps $t$ ($\log_2(t)$, simply the number of bits to represent an integer $t$)
  - Entropy can increase at most logarithmically with $t$

Entropy increasing logarithmically is counterintuitive and a lot slower than expected.

How do we fix this "problem"?

- In the probabilistic case:
  - We must encode both initial state and result at each step, at the least a coin flip (+ $t$)
  - Thus complexity increases at a polynomial rate
- Or we can simply change the definition of this complexity to complexity given a reasonable time complexity cutoff
  - This eliminates the $\log(t)$ solution and thus also makes it polynomial

## How to define "complexity"/"interestingness"

We will refer to complexity as "complextropy" to avoid confusion.

Goal: define a new sophistication function that can measure complextropy:

- Base Kolmogorov complexity cant decipher between random and complex strings

Sophistication helps measure this interestingness:

- Sophistication = complexity of the best model (set of possible $x$'s) that generates $x$, where "best" means that $x$ seems random in it
- Sophistication refers to the simplest model that could have produced $x$, referring to the Kolmogorov complexity of the model itself rather than the $x$

Essentially just how complex the model set is, i.e. random strings themselves would have high complexity, but low sophistication since it can be modeled by `random()`.

As for the math:

- $S$ = set of strings, the "explanation"
- $K(S)$ = how "complex" is this explanation
- $K(x | S)$ = complexity of shortest program that can specify $x$ within $S$

$$K(x|S) \geq \log_2(|S|) - c$$

This is a condition for the sophistication function, essentially in a world such that $x$ seems like a random draw from $S$.

Note that $K(x|S)$ is hard upper bound by $\log_2(|S|)$, and our condition requires that to be close to the max, meaning $x$ must appear "random" within $S$.

This ensures that our model $S$ is a plausible explanation and not "overfit", basically eliminating this overfitting factor from our definition.

**Sophistication:** complexity of the simplest plausible explanation for $x$.


**Goal: find a way to define this idea of complextrophy**

- So far, entropy has failed (grows only $O(\log(t))$, too slow), and sophistication looks promising.

## But for our case, sophistication can never exceed $\log(t) + c$

**In deterministic systems:**

Same issue, simply can define with initial state, transition rule, and time steps. The first two of which are constant, thus $O(\log_2(t) + c)$.

**As for plausibility:**
- For a deterministic system, there's only one possible $S(t)$ thus the RHS is $\le 0$, which our RHS is clearly $\ge 0$ since it is lower bounded by 0.

Okay so in these systems sophistication isn't a reliable way to measure complextrophy either...

**For probabilistic systems:**

Also same issue, but this time defined with the set $S(t)$ of all possible states after $t$ time steps, along with the other two constants of initial state and transition rule.

In this case $x$ would be a particular possible state, and the set being the set of all possible states.

If the probability distribution over $S(t)$ is uniform, the state will almost always be generic and thus satisfy the plausibility clause.

## Complexodynamics

The issue with previous definitions (entropy, sophistication) is that given enough time complextropy does not go back down as hypothesized, even past the equilibrium point. This is because there is no limit on how long an algorithm can run, and it can trade off as much time complexity to minimize number of bits as it wishes.

In this blog the example complexity limit used is $n log(n)$, though this is just an example.

The main principle is that **we impose computation efficiency requirements**, in two places:

- On the sampling algorithm
  - That we used to go from our bits of information to the final state
  - We eliminate the $\log(t) + c$ argument from deterministic, since that runs in $O(t)$
  - Fixes the logarithmic cap
- On the reconstruction algorithm
  - That we used to specify the $x$ via the bits of information we are given
  - An algorithm given infinite time could use some special properties to minimize bits
  - Ensures that complextropy decreases back down

This is the proposed function/definition by the author to measure this, basically sophistication but + time constraints in every step.