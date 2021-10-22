# Heap Sort

{% embed url="https://www.youtube.com/watch?v=B7hVxCmfPtM" %}

## TLDR:

### Runtime: Heapsort has a running time of O(n log n).

Building the max-heap from the unsorted list requires O(n) calls to the `max_heapify` function, each of which takes O(logn) time. Thus, the running time of `build_heap` is O(nlogn).

### Why choose Heapsort?

It is an in-place sort, and has more consistent time performance than quicksort.

{% embed url="https://brilliant.org/wiki/heap-sort/" %}

### Explanation

A binary tree can be represented in an array using the indices:\
The left child node of i = (i \* 2) + 1\
The right child node of i = (i \* 2) + 2\
If the index starts at 0\
\
The algorithm consists of three parts:\
\- Build a max heap from the random array using max-heapify\
\- At this point, the root (arr\[0]) is the largest value. Swap it with the last value of the array and reduce the size of the heap by one. (Imagine that one side is the 'unsorted' max heap, and one side is the sorted array, though they are in the same array.)\
\- Since the root may be 'wrong' now, repeat step 1 and two until the max heap has only one element

{% hint style="info" %}
A max heap isn't perfectly sorted, eg: \[5, 4, 1, 2, 3] is valid.\
A parent node just needs to be bigger than the children nodes.
{% endhint %}

```
let arrLength;

// AKA max-heapify
function heapify(input, i) {
    // treat array as heap
    let leftChild = 2 * i + 1;
    let rightChild = 2 * i + 2;
    let maxIndex = i;

    if (leftChild < arrLength && input[leftChild] > input[maxIndex]) {
        maxIndex = leftChild;
    }
    if (rightChild < arrLength && input[rightChild] > input[maxIndex]) {
        maxIndex = rightChild;
    }
    if (maxIndex != i) {
        [input[i], input[maxIndex]] = [input[maxIndex], input[i]]
        heapify(input, maxIndex);
    }
}

function heapSort(input) {
    arrLength = input.length;

    // build a max heap from bottom-up, which only needs to run arrLength/2 because leaves are already max heaps
    for (let i = Math.floor(arrLength / 2); i >= 0; i -= 1) {
        heapify(input, i);
    }

    // get largest value and place it at the last position of the heap, then reduce heap size iteratively
    for (let i = arrLength - 1; i > 0; i--) {
        [input[0], input[i]] = [input[i], input[0]]
        arrLength--;

        heapify(input, 0);
    }
}
```
