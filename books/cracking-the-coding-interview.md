# Cracking the Coding Interview

## How to calculate space time complexity of an algorithm:

**Run time:** Big O: upper bound/ Big theta: tight bound / Big omega: lower bound For industry use, big O is usually what academics term big theta. Do not confuse them with best case/ worst case and expected case runtime.

**Space complexity** describes the amount of memory required by an algorithm.

**Amortized time** expresses the time complexity of an algorithm that rarely has a ver bad time complexity. \(Eg: ArrayList\) In Big O notation, you can essentially ignore it.

**Drop constants** \(eg: 2 from O\(2N\)\), drop non-dominant terms \(eg: O\(N + log N\) = O\(N\)\)

### Heuristics

**Nested loop with two inputs** =&gt; O\(A_B\) **\(!! different inputs matters\)**_ 

_**Nested loop with same input** =&gt; O\(N \*_2\) 

**Side by side loops** =&gt; O\(A+B\)

**Divide and conquer** =&gt; O\(log N\) 

\(Explanation: Because binary search =&gt; 16 \(step0\), 8\(step1\)... 1\(step4\) is log\(2\)16 = 4, or log\(2\)N\) 

**Recursive runtime** =&gt; tree structure =&gt; \(\(nodes  N+1\) -1\) =&gt; O\(node 2\)

**Sort arr of strs by char in str and str in arr** =&gt; O\(a\*s\(log a + log s\)\) \(!! if N is used differently, use different var\)

## Must know concepts

### Data Structures 

* Linked lists 
* Trees, tries & graphs 
* Stacks & queues 
* Heaps 
* Vectors/ ArrayLists 
* Hash tables\*

### Algorithms 

* Breadth-first search 
* Depth-first search 
* Binary search 
* Merge sort 
* Quick sort

### Concepts 

* Bit manipulation 
* Memory \(Stack vs heap\) 
* Recursion 
* Dynamic programming 
* Big O time & space

## Problem solving flowchart

Listen -&gt; Ask questions -&gt; Exceptions -&gt; Brute force -&gt; Test -&gt; Optimize -&gt; Walk through -&gt; Implement \(Page 62\)

To optimize, look for **BUD** \(Bottlenecks, Unnecessary work, Duplicated work\)

currently at: pg 100\(to be continued\)

