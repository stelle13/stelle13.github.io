---
title: "Recursion I"
date: 2022-03-19
toc: true
mathjax: true
categories:
  - study
tags:
  - recursion
---

# Recursion I

## Tower of Hanoi

$$\text{<Description>}$$

+ Task: 
  + Move a total of $n$ disks of different sizes from the source column to the destination column, using a third column as an intermediary.

+ Rules:
  1. Larger disks may not be placed on top of smaller disks.
  2. One may move one disk at a time. 



$$\text{<Pseudocode>}$$

```
Hanoi (n, source, temporary, destination):
if n > 0
  Hanoi(n-1, source, temporary, destination)
  move bottom disk from source --> destination
  Hanoi(n-1, temporary, destination, source)
```





+ $T(n) = $ Number of moves to transfer $n$ disks 
+ $T(n) = 2 \cdot T(n-1) + 1$

$$$$

$$T(n) = 1+ 2+ 2^2 + ... + 2^{n-1} = \frac{2^n-1}{2-1} \;\;\; \text{(moves)}$$ 



$$\text{<Implementation>}$$


```python
import sys
n = int(sys.stdin.readline()) # number of disks to transfer
# n = 3

steps = 1
for i in range(n - 1):
    steps = steps * 2 + 1
print("Number of Steps to move {} disks: {}".format(n, steps))

def hanoi(num, x, y, z):
    if num > 0:
        hanoi(num-1,x,z,y)
        print("Move disk from column {} to column {}.".format(x,z))
        hanoi(num-1, y,x,z)

hanoi(n,1,2,3)
```

    Number of Steps to move 3 disks: 7
    Move disk from column 1 to column 3.
    Move disk from column 1 to column 2.
    Move disk from column 3 to column 2.
    Move disk from column 1 to column 3.
    Move disk from column 2 to column 1.
    Move disk from column 2 to column 3.
    Move disk from column 1 to column 3.
    
