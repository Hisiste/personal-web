---
title: Twisty Little Passages (Team)
categories: [Technical Logs, Team]
tags: []
math: true
date: 2023-04-24 21:52:01 -0600

author: adriz
---

Try this problem by yourself first! Check the [Code Jam Page](https://codingcompetitions.withgoogle.com/codejam/round/0000000000876ff1/0000000000a45fc0#analysis).

## Overview

We are in a cave system with rooms and passages that connects them. Our task is to estimate the number of passages this cave system has, but we have a lot of drawbacks:

- We can identify the room we're in and how many passages it has, but we cannot distinguish the passages apart. If we walk through a passage, it'll be chosen at random.
- We're only allowed to do up to $K$ operations:
    - Teleport to any room we choose.
    - Walk through a random passage.
- The total number of rooms ($N$) can (and most probably will) be much larger than $K$.

How can we estimate the number of passages in these conditions? With the most simple and naive approach we could possibly think of:

1. Teleport to a random subset of rooms.
1. Record the number of passages each room has.
1. Calculate the average number of passages for a single room.
1. Multiply that average with the total number of rooms and divide by 2.

And that's how we could find our estimate. Although there exist cases where this approach fails, those cases are a bit extreme and will be explained in more detail in the [alternative solutions](#alternative-solutions) section.

For now, we will trust in the testing tool that the Code Jam problem page gives us. It says our solution finds the correct estimate 100% of the time. And that's a lot!

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Context

A cave system is generated randomly. There are $N$ rooms and they are connected through passages. No passage goes from a room to itself, and no two rooms are connected by more than one passage. We can identify each room and see how many passages it has, but we cannot distinguish them apart. Our task is to estimate the number of passages the cave system has by doing up to $K$ operations. An operation can be:

- Teleport to a specified room.
- Walk through a random passage.

If $E$ is our estimate and $P$ the actual number of passages in the cave system, our estimate is considered correct if and only if $P \left( \frac{2}{3} \right) \leq E \leq P \left( \frac{4}{3} \right)$. To successfully pass a test set, at least 90% of the solutions must be correct.

The problem gives us the following restrains: $2 \leq N \leq 10^5$ and $K = 8\,000$. In this sense, we cannot just teleport to every room, since $N$ may be much larger than $K$. What we can do is teleport to a random subset of the rooms and record how many passages each room has. We can even compute the average number of passages for a single room and extrapolate it to all the rooms. That could give us a pretty good estimate.[^1]

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Solution

As described in the [context section](#context), we will compute the average number of passages using only a random subset of the rooms. Let's describe the algorithm.

1. Read $N, K$ from input.
1. Initialize `seen_passages` as an empty list and `rooms_not_seen` as a list of integers from 1 to $N$.
1. We start in a random room. Read $R_0, P_0$ from input, the room we're currently in and its number of passages, respectively.
1. Remove $R_0$ from `rooms_not_seen` and append $P_0$ to `seen_passages`.
1. Repeat for $i \in \\{ 1, 2, \ldots, K \\}$:
    1. Select a random room from `rooms_not_seen` and teleport to it.
    1. Read $R_i, P_i$ from input.
    1. Remove $R_i$ from `rooms_not_seen` and append $P_i$ to `seen_passages`.
1. Compute the average number of passages for a single room:

    $$
    \begin{eqnarray}
    & \text{average} = \frac{\text{sum of }\texttt{seen_passages}}{\text{size of }\texttt{seen_passages}} .
    \end{eqnarray}
    $$
1. Multiply the average of passages with the total number of rooms. Divide the result by 2, because we're counting each passage twice. That's our estimate! :D

    $$
    \begin{eqnarray}
    & \text{result} = \frac{(\text{average}) \cdot N}{2} .
    \end{eqnarray}
    $$

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Alternative Solutions

We have another more sophisticated solution. Instead of just teleporting to random rooms, we alternate teleporting and walking through a random passage. The rooms seen after teleportation are selected uniformly between all rooms, but the ones seen after walking are not. If you're in a room and walk through a passage, there may be rooms that we could never reach.

What we can do is to use the ones seen after teleporting to calculate the average number of passages. We apply the average to the rooms not seen yet and then we add the passages already seen to the result. We finally divide it by two and that would be our estimate.

The biggest advantage of this alternate solution over the main solution is that if there's a case were a few rooms have tons of passages and the rest just barely have passages, this solution would work and the main wouldn't. It'd be easier to find those hyper-connected rooms by randomly walking through a passage from a barely connected room, than teleporting to that room by chance. That'd be a better solution, but unfortunately time is ticking. :'c

---

## Footnotes

[^1]: If we assume that the passages are distributed uniformly. Otherwise, the solution may not work.
