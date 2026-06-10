# Time Complexity & Space Complexity Notes (Interview-Friendly Guide)

# 1. What is Time Complexity?

Time Complexity describes how the amount of work performed by an algorithm grows as the input size increases.

It does **not** measure actual execution time in seconds.

Instead, it answers questions like:

* If the input size doubles, how much more work will the algorithm perform?
* How well does the algorithm scale for large inputs?

Example:

```dart
for (int i = 0; i < n; i++) {
  print(i);
}
```

If:

```text
n = 10   -> 10 iterations
n = 100  -> 100 iterations
n = 1000 -> 1000 iterations
```

The work grows linearly with n.

Therefore:

```text
O(n)
```

---

# 2. What is Space Complexity?

Space Complexity describes how much additional memory an algorithm uses.

Example:

```dart
int sum = 0;
```

Only one variable is used.

```text
O(1)
```

Example:

```dart
Map<String, int> frequency = {};
```

If there are n unique characters, the map may store n entries.

```text
O(n)
```

---

# 3. What is Big O Notation?

The "O" in Big O stands for:

```text
Order of Growth
```

Big O describes how an algorithm's resource usage grows as input size increases.

---

# 4. O(1) — Constant Time

The amount of work remains the same regardless of input size.

Example:

```dart
print(arr[5]);
```

Whether the array contains:

```text
10 elements
1,000 elements
100,000 elements
```

The operation still takes one direct access.

```text
O(1)
```

Interview Explanation:

"The operation does not depend on the size of the input."

---

# 5. O(log n) — Logarithmic Time

Rule:

The problem size is reduced by half in every iteration.

Example:

```dart
while (n > 1) {
  n = n ~/ 2;
}
```

Dry Run:

```text
100
50
25
12
6
3
1
```

The value is halved each time.

```text
O(log n)
```

Interview Explanation:

"Each iteration reduces the problem size by half."

---

# 6. O(n) — Linear Time

Rule:

The entire collection is traversed once.

Example:

```dart
for (int i = 0; i < n; i++) {
}
```

```text
O(n)
```

Examples:

* Traversing an array
* Traversing a string
* Finding maximum value
* Finding minimum value
* Counting occurrences of a character

Interview Explanation:

"The collection is traversed once, so the time complexity is O(n)."

---

# 7. O(n log n)

Commonly seen in efficient sorting algorithms.

Examples:

* Merge Sort
* Heap Sort
* Quick Sort (Average Case)

```text
O(n log n)
```

---

# 8. O(n²) — Quadratic Time

Rule:

Nested loops.

Example:

```dart
for (int i = 0; i < n; i++) {
  for (int j = 0; j < n; j++) {
  }
}
```

Operations:

```text
n × n
```

Result:

```text
O(n²)
```

Examples:

* Bubble Sort
* Selection Sort

Interview Explanation:

"Nested loops result in n² operations."

---

# 9. O(2ⁿ) — Exponential Time

Example:

Naive Recursive Fibonacci

```dart
int fib(int n) {
  return fib(n - 1) + fib(n - 2);
}
```

Complexity:

```text
O(2ⁿ)
```

The number of recursive calls grows exponentially.

---

# 10. O(n!) — Factorial Time

Examples:

* Generating all permutations
* Traveling Salesman (Brute Force)

```text
O(n!)
```

This is one of the slowest complexity classes.

---

# 11. Separate Loops

Example:

```dart
for (int i = 0; i < n; i++) {}

for (int j = 0; j < n; j++) {}
```

Calculation:

```text
O(n + n)
O(2n)
```

Constants are ignored.

Result:

```text
O(n)
```

---

# 12. Nested Loops

Example:

```dart
for (int i = 0; i < n; i++) {
  for (int j = 0; j < n; j++) {}
}
```

Calculation:

```text
n × n
```

Result:

```text
O(n²)
```

---

# 13. Character Frequency Using a Map

```dart
Map<String, int> frequency = {};

for (int i = 0; i < str.length; i++) {
  frequency[str[i]] = (frequency[str[i]] ?? 0) + 1;
}
```

Time Complexity:

```text
O(n)
```

Reason:

The string is traversed once.

Space Complexity:

```text
O(n)
```

Reason:

The map may store up to n unique characters.

---

# 14. Counting Only One Character

```dart
int count = 0;

for (int i = 0; i < str.length; i++) {
  if (str[i] == 'b') {
    count++;
  }
}
```

Time Complexity:

```text
O(n)
```

Space Complexity:

```text
O(1)
```

Reason:

Only one variable is used.

---

# 15. Space Complexity Basics

## O(1) Space

Only a few variables are used.

```dart
int sum = 0;
int max = 0;
```

```text
O(1)
```

---

## O(n) Space

An additional data structure grows with the input.

Examples:

```dart
List<int> temp = [];
Map<String, int> map = {};
Set<int> set = {};
```

```text
O(n)
```

---

## Recursion Space

Example:

```dart
int factorial(int n) {
  if (n <= 1) return 1;

  return n * factorial(n - 1);
}
```

Each recursive call is stored on the call stack.

```text
Time: O(n)
Space: O(n)
```

---

# Quick Interview Cheat Sheet

## Time Complexity

```text
Direct Access             -> O(1)

Reduce by Half            -> O(log n)

Single Traversal          -> O(n)

Efficient Sorting         -> O(n log n)

Nested Loops              -> O(n²)

Recursive Double Calls    -> O(2ⁿ)

Permutations              -> O(n!)
```

---

## Space Complexity

```text
Few Variables             -> O(1)

Extra Array               -> O(n)

Extra Map                 -> O(n)

Extra Set                 -> O(n)

Recursion Stack           -> O(n)
```

---

# Golden Rule

## Time Complexity

Ask:

"How many times is the operation executed?"

## Space Complexity

Ask:

"How much additional memory is allocated?"

---

# Memory Trick

```text
+1 or -1 movement  -> O(n)

×2 or ÷2 movement  -> O(log n)

Nested loops       -> O(n²)

Extra Map/List     -> O(n) Space

Only Variables     -> O(1) Space
```

# Interview Answer Template

Whenever asked about complexity:

1. Count the number of iterations.
2. Check whether the problem size is reduced by half.
3. Look for nested loops.
4. Check whether extra memory structures (List, Map, Set) are used.
5. Mention both Time Complexity and Space Complexity.

Example:

"The array is traversed once, so the time complexity is O(n). No additional data structure is used apart from a few variables, so the space complexity is O(1)."
