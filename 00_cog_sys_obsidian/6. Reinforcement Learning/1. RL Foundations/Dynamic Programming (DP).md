---
tags:
  - rl
---

Dynamic Programming (DP)


DP performs GPI when the model $p(s',r\mid s,a)$ is **known**. Strictly speaking this is planning, not learning: the agent never interacts with the environment, it only computes.

### Gist

Turn the Bellman equation into an assignment and sweep it over every state until the values stop changing:

$$v(s) \leftarrow \sum_a \pi(a\mid s)\sum_{s',r} p(s',r\mid s,a)\big[\,r + \gamma\,v(s')\,\big]$$

The expectation is computed **exactly** by summing over every successor, no sampling involved.

### Why sweeping works

The Bellman equation is a system of $|\mathcal{S}|$ coupled equations, circular by construction. Repeated sweeps propagate value information backwards through the state space, one step per iteration: after one sweep only states adjacent to a reward know anything, after two sweeps states two steps away, and so on until everything is consistent.

### Requirements and limits

| Requirement | Consequence |
| ----------- | ----------- |
| Full model $p$ available | Not applicable to real robots, markets, unknown environments |
| State space enumerable | Fails for Go ($10^{80}$ states) or raw pixels |

A **simulator is not enough**: it returns one sampled outcome per call, while DP needs the full distribution over all successors simultaneously. With only a simulator you are effectively model-free and must use MC or TD.

### Role in the lecture

DP defines what the correct answer is. MC and TD are then understood as ways to approximate the same fixed point when $p$ is unknown.


#### Related
[[1. RL Foundations]]
