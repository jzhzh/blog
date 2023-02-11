---
title: Time and Space Complexity
date: 2022-12-23 21:52:01
tags: 
- leetcode
categories:
- leetcode
- basic algo concepts
---

## Time Complexity

### Defination

Time Complexyity is growth trend of algorithm running **time** as **data size** becomes larger.

### Essence (Optional)

We denote the input data size as $n$. We also denote the number of operations as the function $T(n)$, the time complexity as $O(n)$ and this mathematical notation is called "Big-$O$ Notation", which stands for the "asymptotic upper bound" of the function $T(n)$.

The defination of asymptotic upper bound is as follows:

> If there exists a real number $c$ and a real number $n_0$, we have the formula (1):
> $$
> T(n) \leq c \cdot f(n)
> $$
> Then, we can conclude that $f(n)$ has given a asymptotic upper bound of $T(n)$. Denoting it as the formula (2):
> $$
> T(n) = O(f(n))
> $$
> 

![](https://i.imgur.com/BjKRMsu.png)

Essentially, computing the asymptotic upper bound is **finding a function $f(n)$**, which makes A and B at the same growth level when $n$ tends to infinity.

### How do we find asymptotic upper bound?

We can ignore the coefficients and constant numbers of the number of operations $T(n)$. Hence,

- We should ignore all the operations which are **not** related to $n$. (Constant numbers)
- We should omit all coefficients. (Coefficients)
- We should use multiplication for all nested loops.

The time complexity is determined by the term of highest order in $T(n)$ as follows:

| Number of operations $T(n)$ | Time complexity $O(f(n))$ |
| :-------------------------: | :-----------------------: |
|           100000            |          $O(1)$           |
|           $3n+2$            |          $O(n)$           |
|         $2n^2+3n+2$         |         $O(n^2)$          |
|       $n^3+10000n^2$        |         $O(n^3)$          |
|    $2^n+10000n^{10000}$     |         $O(2^n)$          |

### Common Types

Common time complexity types are (listed from lowest to highest) as formula (3):
$$
O(1) < O(\log n) < O(n) < O(n\log n) < O(n^2) < O(2^n) < O(n!)
$$

<center>constant order < logarithmic order < linear order < linear logarithmic order < squared order < exponential order < factorial order</center>

![](https://i.imgur.com/Pwe2C7e.png)

#### Linear Order $O(n)$

Linear order is often found in the single-level loop. Operations such as **traversing an array** and **traversing a linked list** have a time complexity of $O(n)$.

#### Squared Order $O(n^2)$

The square order is often found in nested loops, where both the outer and inner loops are $O(n)$, nested loops are $O(n^2)$.

Take **bubble sort** as an example, the outer loop has $n-1$ operations, and the inner loop has $n-1, n-2, ..., 2, 1$ operations, which has $\frac{n}{2}$ operations on average. Therefore, the time complexity is $O(n^2)$.

```python
""" bubble sort"""
def bubble_sort(nums):
    count = 0
    # Outer loop: the number of elements to be sorted is n-1, n-2, ..., 1
    for i in range(len(nums) - 1, 0, -1):
        # inner sort: bubble
        for j in range(i):
            if nums[j] > nums[j + 1]:
                # swap nums[j] and nums[j + 1]
                tmp = nums[j]
                nums[j] = nums[j + 1]
                nums[j + 1] = tmp
                count += 3  # 3 operations
    return count
```

#### Exponential Order $O(2^n)$

In biology, **cell division** is exponential growth.

If the time complexity of solving a problem using "violent enumeration" is $O(2^n)$, it is usually necessary to use algorithms such as **dynamic programming** or **greedy algorithm** to solve the problem.

#### Logarithmis Order $O(\log n)$

The logarithmic order is the opposite of the exponential order, which reflects the **increase to twice per round** case, while the former reflects the **decrease to half per round** case. The logarithmic order is second only to the constant order, and the time grows very slowly, which is the ideal time complexity.

The logarithmic order is often found in **binary search** and **Divide and Conquer, D&C**, which reflect the algorithmic ideas of **dividing one into many** and **simplifying the complexity**.

Similar to exponential order, logarithmic order is often found in recursive functions. The following code forms a recursive tree of height $\log_2n$.

```python
def log_recur(n):
    if n <= 1: return 0
    return log_recur(n / 2) + 1
```

#### Linear Logarithmic Order $O(n\log n)$

Linear logarithmic order is often found in nested loops, with time complexity of $O(\log n)$ and $O(n)$ for the two levels, respectively. Mainstream sorting algorithms have a time complexity of $O(n\log n)$, such as quick sort, merge sort, heap sort, etc.

```python
""" Linear logarithmic order """
def linear_log_recur(n):
    if n <= 1: return 1
    count = linear_log_recur(n // 2) + \
            linear_log_recur(n // 2)
    for _ in range(n):
        count += 1
    return count
```

![](https://i.imgur.com/z6imbL1.png)

####  Factorial Order $O(n!)$

Factorials are often implemented using recursion. For example, in the following code, the first level is splited into $n$ nodes, the second layer is splited into $n-1$ nodes, ...... , until it reaches the $n^{th}$ level, the splitting is terminated.

```python
"""Factorial Order"""
def factorial_recur(n):
    if n == 0: return 1
    count = 0
    # One is splited into n nodes
    for _ in range(n):
        count += factorial_recur(n - 1)
    return count
```

![](https://i.imgur.com/5R2ceAL.png)

### Worst, Best, Average Time Complexity

The time complexity of some algorithms is not constant, but is related to the distribution of the input data. For example, if I want to find 1 in an array, the time I spend depends on the position of 1 in the array.

The **asymptotic upper bound of the function** is represented by Big-$O$ notation, which stands for **worst time complexity**. In contrast, the **asymptotic lower bound** is denoted by $\Omega$ notation (Omega Notation), which represents the **best time complexity**.

We seldom use **best time complexity** in practice because it is often achieved only with a small probability and can be misleading. On the contrary, the **worst time complexity** is the most practical because it gives an **efficiency safety value** and allows us to use the algorithm with confidence.

Relatively, the **average time complexity** reflects the efficiency of an algorithm running with random input data, and is represented in $\Theta$ notation (Theta Notation). For the example in paragraph 1, the average time complexity is $\Theta(\frac{n}{2}) = \Theta(n)$.

However, in practice, especially for more complex algorithms, it is difficult to calculate the average time complexity because it is difficult to analyze the overall mathematical expectation under the data distribution in an easy way. In this case, we generally use the worst time complexity as the criterion for judging the efficiency of the algorithm.

### Reference

[hello-algo](https://github.com/krahets/hello-algo) on github. The original author are krahets (github username), etc. This article of the blog is only translated by [hello-algo](https://github.com/krahets/hello-algo).

**This article is for my study, record, and translation only, not for commercial purposes. For infringement, please contact me to delete the article. The original texts, codes, images, photos, and videos in [hello-algo](https://github.com/krahets/hello-algo) are licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/).**
