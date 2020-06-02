# Quick Sort

{% embed url="https://www.youtube.com/watch?v=COk73cpQbFQ" %}

{% embed url="https://brilliant.org/wiki/quick-sort/" %}

### TLDR

Quick sort is a fast sorting algorithm that uses divide-and-conquer, but has a slow worst-case run-time. That possibility can be reduced by picking a correct pivot.

**Run time**  
In average to best case, if pivot is a 'middle' value: n \* Log\(n\)

![](../../../.gitbook/assets/image%20%2837%29.png)

In worst case: O\(n \*\* 2\)

![](../../../.gitbook/assets/image%20%2843%29.png)

### Steps:

**Divide:**

1. [Pick a pivot](https://brilliant.org/wiki/quick-sort/#choosing-a-pivot) element, A\[q\].
2. Partition, or rearrange, the array into two subarrays: A\[p ... q-1\] such that all elements are less than A\[q\], and A\[q+1 … r\] such that all elements are greater than or equal to A\[q\]. \(Eg: if a pivot is the element at midpoint, the subarrays are A\[0 ... midpoint - 1\] and A\[midpoint + 1 ... endpoint\]\)

**Conquer:** Sort the subarrays recursively with quicksort.

**Combine:** No work is needed to combine the arrays because they are already sorted in place. [\[1\]](https://brilliant.org/wiki/quick-sort/#citation-1)



```text
const quickSort = (arr, startIndex = 0, endIndex = arr.length - 1) => {
  if (startIndex < endIndex) {
    const pIndex = partition(arr, startIndex, endIndex);
    quickSort(arr, startIndex, pIndex - 1);
    quickSort(arr, pIndex + 1, endIndex);
  }
  return arr;
};

function partition(arr, startIndex, endIndex) {
  // pick the pivot using median of 3 method
  const midIndex = Math.floor((startIndex + endIndex) / 2);
  if (arr[midIndex] < arr[startIndex]) {
    [arr[midIndex], arr[startIndex]] = [arr[startIndex], arr[midIndex]];
  }
  if (arr[endIndex] < arr[startIndex]) {
    [arr[endIndex], arr[startIndex]] = [arr[startIndex], arr[endIndex]];
  }
  if (arr[midIndex] < arr[endIndex]) {
    [arr[midIndex], arr[endIndex]] = [arr[endIndex], arr[midIndex]];
  }
  const pivot = arr[endIndex];
  // pIndex eventually ends at the pivot's correct position
  let pIndex = startIndex;
  for (let index = startIndex; index < endIndex; index++) {
    if (arr[index] <= pivot) {
      [arr[index], arr[pIndex]] = [arr[pIndex], arr[index]];
      pIndex += 1;
    }
  }
  [arr[pIndex], arr[endIndex]] = [arr[endIndex], arr[pIndex]];
  return pIndex;
}
```

