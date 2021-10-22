# Code Smells

Summary of what I learned from JumpStart's clean code session.

For more on how to write clean code, you can refer to: "Refactoring: Improving the Design of Existing Code" by Martin Fowler.\
\
**1. Mysterious Names**

When: Names are not self-explanatory and descriptive.

Solution: Use clear, descriptive names.

**2. Bad comments**

When: Methods filled with explanatory comments that are unnecessary.

Solution: Code should strive to be self-explanatory. Comment the pitfalls and not the obvious.

**3. Complex conditions**

When: There are long expressions that are hard to read.

Solution: Simplify with [De Morgan's Law.](https://erikmhsiao.github.io/de-morgans-laws/)

**4. Don't use "else" when unnecessary**

When: Code looks like a Christmas Tree.

Solution: Early returns.

**5. Double negatives**

Why: Double negatives are difficult to read.

**6. Loopy code**

Solution: Use .map, .filter, .reduce instead.

**7. Duplicate code**

Solution: Refactor out common functions.

**8. Dead code**

When: Code that is not used because it's obsolete.

Solution: Remove greyed out code.

**9. Overly long function**

When: Methods are more than 10 lines.

Solution: Follow the single responsibility principle.

**10. Long parameter lists**

Solution: Extract into class methods.

**11. God class**

When: Class has too many fields, methods or/and lines of code.

**12. Speculative Generality**

When: Unused code for "what ifs".

Solution: If in doubt, YAGNI (You probably Aren't Gonna Need It).

**13. Primitive Obsession**

When: A lot of primitives instead of objects. (Eg: admin\_role = 1)

Solution: Wrap primitives in classes.

**Resources:**

[https://blog.codinghorror.com/code-smells/](https://blog.codinghorror.com/code-smells/)\
[https://refactoring.guru/refactoring/smells](https://refactoring.guru/refactoring/smells)\
[https://williamdurand.fr/2013/06/03/object-calisthenics](https://williamdurand.fr/2013/06/03/object-calisthenics)\
[https://hackernoon.com/lessons-learned-common-react-code-smells-and-how-to-avoid-them-f253eb9696a4](https://hackernoon.com/lessons-learned-common-react-code-smells-and-how-to-avoid-them-f253eb9696a4)\
[https://github.com/Droogans/unmaintainable-code](https://github.com/Droogans/unmaintainable-code)
