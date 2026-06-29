---
title: "A Tiny Q-learning Example in Python"
date: 2026-06-29T08:30:00+05:30
draft: false
tags: ["reinforcement learning", "python", "q-learning"]
---

I wanted to get the temporal-difference update rule into my fingers rather than
just nod along to it, so I wrote the smallest Q-learning program I could. No
dependencies, just the standard library. The code lives in
[`q-learning`](https://github.com/bharathbhushan1/ai-learning/tree/main/q-learning),
managed with [uv](https://docs.astral.sh/uv/).

![A 1D line of six cells, 0 through 5; the agent starts at cell 0 and the learned policy points right at every step toward the goal at cell 5, with Q-values discounted back from the goal](images/q-learning.svg)

## The world

A 1D line of six cells `[0, 1, 2, 3, 4, 5]`. The agent starts at cell 0 and must
reach the goal at cell 5. Each step it moves **left** or **right** (clamped at
the ends). Reaching the goal pays `+1` and ends the episode; every other step
pays `0`. That's the entire environment — `env_step` is six lines.

## The learning

`Q[state][action]` estimates how good each action is in each state. After every
step the table is nudged toward the temporal-difference target:

```
Q(s, a) <- Q(s, a) + alpha * (reward + gamma * max_a' Q(s', a') - Q(s, a))
```

The one detail worth slowing down on is that Q-learning is **off-policy**, and
the code makes that split explicit with two named functions:

- `behaviour_policy` — how the agent *actually moves*: epsilon-greedy, mostly
  greedy but occasionally random so it keeps exploring.
- `target_policy` — the value that *bootstraps the update*: `max(Q[next_state])`,
  the best action in the next state.

The agent behaves one way (epsilon-greedy) but learns about another (the greedy
policy). That `max` is the whole trick.

## The result

After 200 episodes the best action in every non-goal state is `right`, and the
learned values fall off from the goal by a factor of `gamma` each step —
exactly the discounted distance to the reward:

```
state 0: right
state 1: right
state 2: right
state 3: right
state 4: right
state 5: GOAL
```

Six cells is trivially solvable by hand, which is the point: when the answer is
obvious, you can see the algorithm arrive at it and trust that the machinery is
doing what you think it is.
