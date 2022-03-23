---
title: "Graph Theory I"
date: 2022-02-25
toc: true
mathjax: true
categories:
  - study
tags:
  - graph theory
---

In this post, I cover the basics of graph theory, including graph representations and simple graph search methods. In writing this post, I referenced my own study notes on Jeff Erickson's textbook, [*Algorithms*](https://jeffe.cs.illinois.edu/teaching/algorithms/book/Algorithms-JeffE.pdf), which is available online under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/), as well as Youtube [lectures](https://www.youtube.com/watch?v=oDqjPvD54Ss&t=153s) by [William Fiset](https://www.youtube.com/watch?v=oDqjPvD54Ss&t=153s) and professor [Erik Demaine](https://www.youtube.com/watch?v=s-CYnVz-uh4).

# Basics

## Graph Notation

+ Graph $(V,E)$ = Pair of sets $V, E$
+ V = Set of nodes
+ E = Set of edges between two nodes
  + $E \subseteq \{\{x, y\} \;\;|\;\;  x,y \in V, x \neq y\}$



## Terms & Definitions

+ Degree of Node: 
  + Number of adjacent nodes
+ Subgraph:
  + $G' = (V', E'), \;\; (V' \subseteq V, W' \subseteq W)$
+ Types of Graphs:
  + Directed graph:
    + In-degree: The number of nodes leading to the node
    + Out-degree: The number of nodes leading out of the node
  + Undirected graph: 
  + Closed graph: A graph that starts and ends on same node
  + Cycle: A particular closed graph that has only one entry point
  + Others: Forests, Trees, etc





## Types of Graph Representations

+ Adjacency List:
  + A group of connected lists representing a graph, where each connected list contains all nodes adjacent to particular index node. 

+ Adjacency Matrix:
  + A symmetric matrix where each element describes the connectivity of two nodes in a finite graph. 


## Types of Graph Searches / Traversals

$$\text{Depth-first Search}$$

```
dfs(node = k):
  if k NOT marked:
    mark k
    for each m adjacent to k:
      dfs(m)
```
$$$$

$$\text{Breadth-first Search}$$

```
bfs(G, s):
    s.dist = 0
    s.pred = NULL
    s.color = GRAY
    for all vertices v != s
        v.color = WHITE
        v.dist = ∞
        v.pred = NULL
    PUSH(s)
    while Q is not empty
        u = DEQUEUE(Q)
        for all edges v adjacent to u
            if v.dist > u.dist + 1
                v.dist = u.dist + 1
                v.pred = u
                v.color = GRAY
                PUSH(v)
        u.color = BLACK
```


# Exercise

Source: USA Computing Olympiad US Open 2007 Contest, Silver No. 2


<figure class="align-center">
  <img src="/assets/images/john1.png" alt="">
</figure> 



Input

<figure class="align-center">
  <img src="/assets/images/john2.png" alt="">
</figure> 


Output

<figure class="align-center">
  <img src="/assets/images/john3.png" alt="">
</figure> 


Hint

<figure class="align-center">
  <img src="/assets/images/john4.png" alt="">
</figure> 


Solution Code


```python
import sys

def bfs(start, target, dist):
    queue = []
    queue.append(start)
    while len(queue) != 0:
        u = queue.pop(0)
        if u == target:
            print(dist[u])
            break
        for v in [u-1, u+1, 2*u]:
            if (0 <= v <= 100000) and not dist[v]:
                queue.append(v)
                dist[v] = dist[u] + 1

input = sys.stdin.readline()

s,t = map(int, input.split())
dist = [0] * 100001
bfs(s,t, dist)
```

# Conclusion

Over the past few weeks, I have taken some time each day to preview course materials for a class called "Algorithms and Data Structures," offered by my university. Admittedly, the focus of this blog has diverged from ML/DL to a somewhat broader and less specific field, including recursion and graph theory. Nonetheless, I expect this blog to be a hodgepodge mix of various topics in computer science and mathematics in the near future, with a diversity of topics and discussions. In the next post, I hope to discuss either graph theory with an in-depth focus on more advanced subtopics, or other concepts such as network flows, dynamic programming, greedy algorithms, and etc.  
