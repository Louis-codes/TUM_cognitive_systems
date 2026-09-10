---
tags:
  - rl
---

Generalized Policy Iteration (GPI)


GPI is not an algorithm but the **schema** that all RL methods follow: alternate between making the values honest and making the policy greedy. DP, MC and TD are instantiations that differ only in how the evaluation step is carried out.

### The two steps

**1. Policy evaluation** — how good is my current policy? Solve the Bellman expectation equation until the values stop changing:

$$v_\pi(s) = \sum_a \pi(a\mid s)\sum_{s',r} p(s',r\mid s,a)\big[\,r + \gamma\,v_\pi(s')\,\big]$$

**2. Policy improvement** — act greedily with respect to the values just computed:

$$\pi'(s) = \arg\max_a q_\pi(s,a)$$

The new policy is provably at least as good as the old one, but the values now belong to the outdated policy. So evaluate again, improve again, and repeat.

### Termination

The loop stops when greedy improvement changes nothing, i.e. when

$$v_\pi(s) = \max_a q_\pi(s,a) \quad \text{for all } s$$

This is exactly the **Bellman optimality equation**. Reaching it means $v_\pi = v_*$ and $\pi = \pi_*$.

| Equation | Role in GPI |
| -------- | ----------- |
| Bellman expectation | What your current policy is worth (evaluation) |
| Bellman optimality | What the best policy is worth (the fixed point) |

### Why "generalized"

Neither step has to run to completion before switching. Value iteration is the extreme case: a single evaluation sweep with the $\max$ folded in, then immediately improve, which merges both steps into one update.

### $v$ or $q$?

Decided by whether the model $p$ is known.

| Setting | Stored | Improvement step |
| ------- | ------ | ---------------- |
| Model known (DP) | $v$ | Reconstruct $q_\pi(s,a) = \sum_{s',r} p(s',r\mid s,a)[r + \gamma v_\pi(s')]$ by lookahead |
| Model unknown (MC, TD) | $q$ | Read off $\arg\max_a q(s,a)$ directly |

Improvement is always defined via $q$; the only question is whether it is computed or stored.

### Exploration

$\arg\max$ alone only ever tries the action currently believed best, so better alternatives are never discovered. Hence the policy stays stochastic during learning (e.g. $\varepsilon$-greedy), even when the final policy is deterministic.


#### Related
[[1. RL Foundations]]
