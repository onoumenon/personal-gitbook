# Cracking the Coding Interview

## How to calculate space time complexity of an algorithm:

**Run time:** Big O: upper bound/ Big theta: tight bound / Big omega: lower bound For industry use, big O is usually what academics term big theta. Do not confuse them with best case/ worst case and expected case runtime.

**Space complexity** describes the amount of memory required by an algorithm.

**Amortized time** expresses the time complexity of an algorithm that rarely has a ver bad time complexity. (Eg: ArrayList) In Big O notation, you can essentially ignore it.

**Drop constants** (eg: 2 from O(2N)), drop non-dominant terms (eg: O(N + log N) = O(N))

### Heuristics

**Nested loop with two inputs** => O(A_B) **(!! different inputs matters)** _&#x20;

_**Nested loop with same input** => O(N \*_2)&#x20;

**Side by side loops** => O(A+B)

**Divide and conquer** => O(log N)&#x20;

(Explanation: Because binary search => 16 (step0), 8(step1)... 1(step4) is log(2)16 = 4, or log(2)N)&#x20;

**Recursive runtime** => tree structure => ((nodes  N+1) -1) => O(node 2)

**Sort arr of strs by char in str and str in arr** => O(a\*s(log a + log s)) (!! if N is used differently, use different var)

## Must know concepts

### Data Structures&#x20;

* Linked lists&#x20;
* Trees, tries & graphs&#x20;
* Stacks & queues&#x20;
* Heaps&#x20;
* Vectors/ ArrayLists&#x20;
* Hash tables\*

### Algorithms&#x20;

* Breadth-first search&#x20;
* Depth-first search&#x20;
* Binary search&#x20;
* Merge sort&#x20;
* Quick sort

### Concepts&#x20;

* Bit manipulation&#x20;
* Memory (Stack vs heap)&#x20;
* Recursion&#x20;
* Dynamic programming&#x20;
* Big O time & space

## Problem solving flowchart

Listen -> Ask questions -> Exceptions -> Brute force -> Test -> Optimize -> Walk through -> Implement (Page 62)

To optimize, look for **BUD** (Bottlenecks, Unnecessary work, Duplicated work)

currently at: pg 100(to be continued)
