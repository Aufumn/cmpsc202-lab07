# Lab 07: The Integer-Valued Knapsack Problem

**Name:** Autumn
## Introduction
In this lab, you will explore different variations of the Knapsack problem and design a dynamic programming solution for one of them. You may work in small groups or individually, but everyone should write up their own answers to the questions below. 

Suppose we're going hiking. We have items $1, \ldots, n$ that we could pack in multiples with corresponding values $v_1, \ldots, v_n$ and weights $w_1, \ldots, w_n$. For example, we might have a water bottle (weight 1, value 2) and a sandwich (weight 2, value 5). Consider the following three problem variations:

1. Maximize value subject to capacity $W$, where you can pack at most one of each item.
2. Maximize value subject to capacity $W$, where you can pack a positive integer value of each item.
3. Minimize weight subject to minimum value $V$, where you can pack a positive integer value of each item.

---

## Part 1: Dynamic Programming Fundamentals

**Question:** Which of the three problem variations can be solved using the dynamic programming approaches we've studied in lecture? Explain your reasoning.

*Your Answer:* All three problems can be solved using dynamic programming because they have optimal substructure and overlapping subproblems.
1. This is the 0/1 Knapsack problem. Each item can be selected at most once. Dynamic programming can solve it by considering whether to include or exclude each item.
2. This is the Integer-Valued Knapsack problem. Items can be selected multiple times. Dynamic programming can solve it by considering each item and the remaining capacity.
3. This is the minimum-weight Knapsack problem. Dynamic programming can solve it by finding the minimum weight needed to reach a required value.

## Part 2: The Naive Recursive Algorithm

Below is a naive recursive algorithm intended to solve one of the variations listed above.

```text
Function MaxValue(W):
    if W == 0:
        return 0
        
    max_val = 0
    for i from 1 to n:
        if w_i <= W:
            current_val = v_i + MaxValue(W - w_i)
            if current_val > max_val:
                max_val = current_val
                
    return max_val
```

**Question:** Which of the three problem variations (1, 2, or 3) does this algorithm solve? How do you know? Explain your reasoning.

*Your Answer:* The algorithm solves the second variation by maximizing value subject to capacity W, where you can pack a positive integer number of each item.
The algorithm allows an item to be selected multiple times because it calls MaxValue(W - w_i) after selecting an item. It does not keep track of which items have already been selected, so the same item can be chosen again. It maximizes the total value while keeping the weight within the capacity.

## Part 3: Recursion Tree

Consider the following simple example:
* **Knapsack Capacity:** $W = 3$
* **Item 1:** Weight $w_1 = 1$, Value $v_1 = 2$
* **Item 2:** Weight $w_2 = 2$, Value $v_2 = 5$
* **Item 3:** Weight $w_3 = 1$, Value $v_3 = 3$

**Question:** Draw the full recursion tree for `MaxValue(3)` using the example above. Label the remaining capacity at each node and indicate which item is chosen along each branch.

*Your Answer (take a picture and upload it):*


## Part 4: Recurrence Relation

**Question:** Derive a mathematical recurrence relation for $OPT(W)$, the maximum value you can pack into a knapsack of capacity $W$, based on the naive recursive algorithm. Be sure to state your base case(s) and explain how you arrived at your formula.

*Your Answer:* The base case is when the capacity is 0, because no more items can be packed and the maximum value is 0.

For any positive capacity, we can consider every item that fits. We add its value to the optimal value of the remaining capacity. We choose the item that gives us the greatest total value.


## Part 5: Bottom-Up Dynamic Programming

Instead of using the slow recursive approach, we can build a table bottom-up. 

**Question:** Use your recurrence relation from Part 4 to fill in the following table for the example provided in Part 3 ($w_1=1, v_1=2$ and $w_2=2, v_2=5$). At each step, record the optimal value and the specific item chosen to achieve that value. 

| Capacity ($W$) | Optimal Value ($OPT$) | Item Chosen |
|:---:|:---|:---|
| **0** | 0 | None |
| **1** | 2 | Item 1 |
| **2** | 5 | Item 2 |
| **3** | 7 | Item 1 |
| **4** | 10 | Item 2 |
| **5** | 12 | Item 1 |
| **6** | 15 | Item 2 |

*Explain how you calculated the row for $W=6$*


## Part 6: Traceback

**Question:** Using the "Item Chosen" column from the table you completed in Part 5, trace back to find the exact quantities of each item to pack that maximize the value. What is the final optimal packing configuration for $W=6$ and the maximum value?

*Your Answer:* Starting at capacity 6, the table says to choose Item 2, which has weight 2. This leaves a capacity of 4. At capacity 4, choose Item 2 again, leaving a capacity of 2. At capacity 2, choose Item 2 again, leaving a capacity of 0. this leaves the optimal configuration as Item 1: 0 Item 2: 3 Item 3: 0. Maximum value: 15. Total weight: 6.
