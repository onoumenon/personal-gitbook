# algos

## recursion

```
export default function factorialRecursive(number) {
  return number > 1 ? number * factorialRecursive(number - 1) : 1;
}
```

## greedy

```
/**
 * GREEDY approach of solving Jump Game.
 *
 * This comes out as an optimisation of DYNAMIC PROGRAMMING BOTTOM_UP approach.
 *
 * Once we have our code in the bottom-up state, we can make one final,
 * important observation. From a given position, when we try to see if
 * we can jump to a GOOD position, we only ever use one - the first one.
 * In other words, the left-most one. If we keep track of this left-most
 * GOOD position as a separate variable, we can avoid searching for it
 * in the array. Not only that, but we can stop using the array altogether.
 *
 * We call a position in the array a "good" one if starting at that
 * position, we can reach the last index. Otherwise, that index
 * is called a "bad" one.
 *
 * @param {number[]} numbers - array of possible jump length.
 * @return {boolean}
 */
export default function greedyJumpGame(numbers) {
  // The "good" cell is a cell from which we may jump to the last cell of the numbers array.

  // The last cell in numbers array is for sure the "good" one since it is our goal to reach.
  let leftGoodPosition = numbers.length - 1;

  // Go through all numbers from right to left.
  for (let numberIndex = numbers.length - 2; numberIndex >= 0; numberIndex -= 1) {
    // If we can reach the "good" cell from the current one then for sure the current
    // one is also "good". Since after all we'll be able to reach the end of the array
    // from it.
    const maxCurrentJumpLength = numberIndex + numbers[numberIndex];
    if (maxCurrentJumpLength >= leftGoodPosition) {
      leftGoodPosition = numberIndex;
    }
  }

  // If the most left "good" position is the zero's one then we may say that it IS
  // possible jump to the end of the array from the first cell;
  return leftGoodPosition === 0;
}
```

{% embed url="https://brilliant.org/wiki/greedy-algorithm#:~:text=A%20greedy%20algorithm%20is%20a,to%20solve%20the%20entire%20problem.&text=However%2C%20in%20many%20problems%2C%20a,not%20produce%20an%20optimal%20solution." %}



If both of the properties below are true, a greedy algorithm can be used to solve the problem.

* **Greedy choice property:** A global (overall) optimal solution can be reached by choosing the optimal choice at each step.
* **Optimal substructure:** A problem has an optimal substructure if an optimal solution to the entire problem contains the optimal solutions to the sub-problems.

In problems where greedy algorithms fail, [dynamic programming](https://brilliant.org/wiki/problem-solving-dynamic-programming/) might be a better approach.

## Dynamic Programming

**Dynamic programming** refers to a problem-solving approach, in which we precompute and store simpler, similar subproblems, in order to build up the solution to a complex problem.

{% embed url="https://brilliant.org/wiki/problem-solving-dynamic-programming" %}

## Divide and Conquer

{% embed url="https://brilliant.org/wiki/divide-and-conquer" %}
