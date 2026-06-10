# Time Complexity & Space Complexity (Complete Guide)

**Beginner-Friendly • Interview-Ready • Practical Examples**

---

## Table of Contents

1. [Why Should You Care About Complexity?](#why-care)
2. [Time Complexity Explained](#time-complexity)
3. [Space Complexity Explained](#space-complexity)
4. [Big O Notation Simplified](#big-o)
5. [Complete Complexity Guide](#complexity-guide)
6. [Common Mistakes](#mistakes)
7. [Real-World Analogies](#analogies)
8. [Practice Problems](#practice)
9. [Interview Answers](#interviews)
10. [Quick Reference](#reference)

---

## Why Should You Care About Complexity? {#why-care}

### Real Impact on Performance

Imagine you have **1 million users** of your app:

| Complexity | Time for 1M items | Speed |
|-----------|------------------|--------|
| O(1) | < 1ms | ⚡ Lightning Fast |
| O(log n) | ~20ms | 🚀 Very Fast |
| O(n) | ~1 second | ✅ Good |
| O(n log n) | ~20 seconds | 📊 Acceptable |
| O(n²) | 11 DAYS | 🐌 Unacceptable |
| O(2ⁿ) | 300,000 YEARS | 💀 Never Completes |

**One bad algorithm can break your entire app!**

---

## Time Complexity Explained {#time-complexity}

### Simple Definition

**Time Complexity** = How the number of operations grows as input size grows.

It's NOT about actual seconds—it's about the **pattern of growth**.

### Visual Comparison

```
Input Size: 10, 100, 1000, 10000

O(1):      10 ops, 10 ops, 10 ops, 10 ops           [Same work always]
O(log n):  3 ops, 7 ops, 10 ops, 13 ops             [Tiny increase]
O(n):      10 ops, 100 ops, 1000 ops, 10000 ops     [Linear increase]
O(n²):     100 ops, 10000 ops, 1M ops, 100M ops     [Huge increase]
```

### Quick Rule Finder

**Ask yourself:** "If input doubles, what happens?"

- **Work stays same?** → O(1)
- **Work doubles?** → O(n)
- **Work halves?** → O(log n)
- **Work quadruples?** → O(n²)

---

## Space Complexity Explained {#space-complexity}

### Simple Definition

**Space Complexity** = How much extra memory an algorithm needs.

**Important:** We count EXTRA memory (not the input itself).

### Visual Comparison

```
Using 10 extra variables?  → O(1) ✅
Using 1 extra array?       → O(n) 📦
Using 2 extra arrays?      → O(2n) = O(n) 📦📦
Using a map with n items?  → O(n) 📦
```

---

## Big O Notation Simplified {#big-o}

### What Does "O" Mean?

```
O = Order of Growth = How fast does work grow?
```

### The Big O Family (From Fast to Slow)

```
O(1)      ⚡ Best
O(log n)  🚀 Excellent
O(n)      ✅ Good
O(n log n) 📊 Okay
O(n²)     🤔 Slow
O(n³)     😞 Very Slow
O(2ⁿ)     💀 Unacceptable
O(n!)     🔥 Never Use
```

### Why Ignore Constants?

```dart
// This is O(3n) but we call it O(n)
for (int i = 0; i < n; i++) {
  operation1();  // 1x
  operation2();  // 1x
  operation3();  // 1x
}

// This is O(10n) but we also call it O(n)
for (int i = 0; i < n; i++) {
  for (int j = 0; j < 10; j++) {
    operation();  // 10x
  }
}

// Why? Because with large n, the constant doesn't matter:
// 3n vs 10n vs n:  When n=1000000, all are huge, constants are negligible
```

---

## Complete Complexity Guide {#complexity-guide}

### 1️⃣ O(1) — Constant Time

**Definition:** The work is always the same, no matter the input size.

#### Real-World Analogy
Picking any book from a specific shelf in your room. It takes the same time whether you have 10 books or 10,000 books.

#### Code Examples

```dart
// Example 1: Array Access
List<int> arr = [10, 20, 30, 40, 50];
print(arr[0]);  // O(1) - Direct access

// Example 2: Simple Calculation
int add(int a, int b) {
  return a + b;  // O(1) - Always 1 operation
}

// Example 3: Dictionary Lookup
Map<String, int> ages = {'Alice': 25, 'Bob': 30};
int aliceAge = ages['Alice']!;  // O(1) - Direct lookup
```

#### Time Visualization
```
n = 10:      ▓ 1 operation
n = 100:     ▓ 1 operation
n = 1000:    ▓ 1 operation
n = 10000:   ▓ 1 operation
```

#### Interview Answer
> "This is a direct operation that doesn't depend on input size. Whether we access element 0 or the last element, it's the same speed: **O(1)**."

---

### 2️⃣ O(log n) — Logarithmic Time

**Definition:** Problem size reduces by half in each iteration.

#### Real-World Analogy
📖 Finding a name in a phonebook: You don't check every page. You open it halfway, see if you need to go forward or backward, then repeat. This is **Binary Search**.

#### Code Examples

```dart
// Example 1: Binary Search
int binarySearch(List<int> arr, int target) {
  int left = 0, right = arr.length - 1;
  
  while (left <= right) {
    int mid = (left + right) ~/ 2;
    
    if (arr[mid] == target) return mid;
    else if (arr[mid] < target) left = mid + 1;  // Search right half
    else right = mid - 1;                         // Search left half
  }
  return -1;
}
// Time: O(log n) - Each step eliminates half the array
```

```dart
// Example 2: Halving a Number
int countHalves(int n) {
  int count = 0;
  while (n > 1) {
    n = n ~/ 2;  // Cut in half each time
    count++;
  }
  return count;
}
// For n=100: 100→50→25→12→6→3→1 = 7 steps
// For n=1000: Takes ~10 steps (not 1000!)
```

#### Time Visualization
```
n = 10:      ▓▓▓ ~3 operations (log₂ 10 ≈ 3.3)
n = 100:     ▓▓▓▓▓▓▓ ~7 operations (log₂ 100 ≈ 6.6)
n = 1000:    ▓▓▓▓▓▓▓▓▓▓ ~10 operations (log₂ 1000 ≈ 10)
n = 1000000: ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ ~20 operations (log₂ 1M ≈ 20)
```

#### Interview Answer
> "Binary search divides the problem in half each iteration. For 1 million items, it only needs ~20 steps. That's **O(log n)** — super efficient!"

---

### 3️⃣ O(n) — Linear Time

**Definition:** You touch each element exactly once.

#### Real-World Analogy
📋 Reading every name on a guest list. You start at top, read each name once, reach bottom. Simple!

#### Code Examples

```dart
// Example 1: Find Maximum
int findMax(List<int> arr) {
  int max = arr[0];
  
  for (int i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
      max = arr[i];  // Touch each element once
    }
  }
  return max;
}
// Time: O(n) - Visit each element once

// Example 2: Count Character Frequency
int countChar(String str, String char) {
  int count = 0;
  
  for (int i = 0; i < str.length; i++) {
    if (str[i] == char) {
      count++;  // Touch each character once
    }
  }
  return count;
}
// Time: O(n) - Visit each character once

// Example 3: Sum All Elements
int sumArray(List<int> arr) {
  int sum = 0;
  for (int i = 0; i < arr.length; i++) {
    sum += arr[i];  // Touch each element once
  }
  return sum;
}
// Time: O(n)
```

#### Time Visualization
```
n = 10:      ▓▓▓▓▓▓▓▓▓▓ 10 operations
n = 100:     ▓▓▓▓▓... [100 bars] 100 operations
n = 1000:    ▓▓▓▓▓... [1000 bars] 1000 operations
```

#### Interview Answer
> "We traverse the array once, touching each element. No matter what we do inside the loop, we visit n elements: **O(n)**."

---

### 4️⃣ O(n log n) — The Sorting Sweet Spot

**Definition:** Efficient sorting algorithms (Merge Sort, Heap Sort, Quick Sort average).

#### Why This Matters
🎯 **O(n log n)** is the fastest comparison-based sorting is possible.

#### Code Examples

```dart
// Example 1: Merge Sort (O(n log n))
List<int> mergeSort(List<int> arr) {
  if (arr.length <= 1) return arr;
  
  int mid = arr.length ~/ 2;
  List<int> left = mergeSort(arr.sublist(0, mid));      // Divide
  List<int> right = mergeSort(arr.sublist(mid));        // Divide
  
  return merge(left, right);  // Conquer - O(n) merge
}
// Divisions: O(log n)
// Merges at each level: O(n)
// Total: O(n log n)

// Example 2: Using Dart's built-in sort
List<int> numbers = [5, 2, 8, 1, 9];
numbers.sort();  // O(n log n) - Built-in uses Quicksort variant
```

#### Time Visualization
```
n = 10:      ▓▓▓▓▓▓▓ ~33 operations (10 × 3.3)
n = 100:     ▓▓▓▓▓▓... ~660 operations (100 × 6.6)
n = 1000:    ▓▓▓▓▓▓... ~10000 operations (1000 × 10)
```

#### Interview Answer
> "Merge Sort divides the array into halves (O(log n)) and merges them back (O(n)). Total: **O(n log n)** — best for sorting!"

---

### 5️⃣ O(n²) — Quadratic Time

**Definition:** Nested loops touching each element multiple times.

#### Real-World Analogy
🎓 Checking if any two students have the same birthday:
```
Student 1 compared with: 2, 3, 4, 5... (n comparisons)
Student 2 compared with: 3, 4, 5...   (n-1 comparisons)
Student 3 compared with: 4, 5...     (n-2 comparisons)
...
Total ≈ n × n = n²
```

#### Code Examples

```dart
// Example 1: Bubble Sort
void bubbleSort(List<int> arr) {
  int n = arr.length;
  
  for (int i = 0; i < n; i++) {           // Outer loop: n times
    for (int j = 0; j < n - i - 1; j++) { // Inner loop: n times
      if (arr[j] > arr[j + 1]) {
        // Swap
        int temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
  }
}
// Time: O(n²) - Nested loops

// Example 2: Find Duplicates (Brute Force)
bool hasDuplicate(List<int> arr) {
  for (int i = 0; i < arr.length; i++) {           // Outer loop
    for (int j = i + 1; j < arr.length; j++) {     // Inner loop
      if (arr[i] == arr[j]) {
        return true;  // Found duplicate
      }
    }
  }
  return false;
}
// Time: O(n²) - Every element compared with every other

// Example 3: Matrix Operations
void printMatrix(List<List<int>> matrix) {
  int rows = matrix.length;
  int cols = matrix[0].length;
  
  for (int i = 0; i < rows; i++) {      // Outer loop
    for (int j = 0; j < cols; j++) {    // Inner loop
      print(matrix[i][j]);
    }
  }
}
// Time: O(n²) - Visit each cell once (n × n matrix)
```

#### Time Visualization
```
n = 10:      ▓▓▓▓▓... 100 operations (10 × 10)
n = 100:     ▓▓▓▓▓... 10,000 operations (100 × 100)
n = 1000:    ▓▓▓▓▓... 1,000,000 operations (1000 × 1000)
n = 10000:   ▓▓▓▓▓... 100,000,000 operations 😱
```

#### Interview Answer
> "We have two nested loops, each running n times. This means O(n) × O(n) = **O(n²)**. With 10,000 items, we get 100 million operations—slow!"

---

### 6️⃣ O(2ⁿ) — Exponential Time

**Definition:** Doubling work for each additional input (recursion without memoization).

#### Real-World Analogy
🌳 Family tree: Each person has 2 parents. Tracing your family tree:
```
You: 1 person
Parents: 2 people
Grandparents: 4 people
Great-grandparents: 8 people
...
Generation n: 2ⁿ people
```

#### Code Examples

```dart
// Example 1: Fibonacci (SLOW - No Memoization)
int fib(int n) {
  if (n <= 1) return n;
  
  return fib(n - 1) + fib(n - 2);  // Two recursive calls!
}

// Call tree for fib(5):
//           fib(5)
//         /        \
//      fib(4)      fib(3)
//     /     \      /     \
//   fib(3)  fib(2) fib(2) fib(1)
//   ...many more calls...
//
// Total calls ≈ 2⁵ = 32 calls!
// For fib(40): ~1 billion calls 💀

// Example 2: Generate All Subsets
List<List<int>> generateSubsets(List<int> arr) {
  List<List<int>> result = [];
  int n = arr.length;
  
  for (int i = 0; i < (1 << n); i++) {  // 2ⁿ iterations
    List<int> subset = [];
    for (int j = 0; j < n; j++) {
      if ((i & (1 << j)) != 0) {
        subset.add(arr[j]);
      }
    }
    result.add(subset);
  }
  return result;
}
// For n=5: 2⁵ = 32 subsets
// For n=20: 2²⁰ = 1,048,576 subsets 😱
// For n=30: 2³⁰ = 1 billion subsets 💀

// Example 3: All Permutations (Also O(2ⁿ) class)
// Generating all orderings of n items
```

#### Time Visualization
```
n = 10:      ▓▓▓... 1,024 operations
n = 15:      ▓▓▓... 32,768 operations
n = 20:      ▓▓▓... 1,048,576 operations
n = 25:      ▓▓▓... 33,554,432 operations
n = 30:      ▓▓▓... 1,073,741,824 operations 🔥
n = 40:      ▓▓▓... 1,099,511,627,776 operations 💀
```

#### Interview Answer
> "Without memoization, each call spawns 2 more calls. Work doubles with each additional input: **O(2ⁿ)**. Avoid this at all costs for large n!"

#### How to Optimize

```dart
// ✅ FIXED: Use Memoization
Map<int, int> memo = {};

int fib(int n) {
  if (n <= 1) return n;
  if (memo.containsKey(n)) return memo[n]!;
  
  int result = fib(n - 1) + fib(n - 2);
  memo[n] = result;
  return result;
}
// Now it's O(n) instead of O(2ⁿ)! Each value calculated once.
```

---

### 7️⃣ O(n!) — Factorial Time

**Definition:** Trying every possible arrangement or permutation.

#### Real-World Analogy
🎲 Arranging students in a line:
```
1 student: 1 arrangement
2 students: 2 arrangements
3 students: 6 arrangements
4 students: 24 arrangements
5 students: 120 arrangements
n students: n! arrangements
```

#### Code Examples

```dart
// Example 1: Generate All Permutations
List<List<int>> permutations(List<int> arr) {
  List<List<int>> result = [];
  
  void backtrack(List<int> current, List<int> remaining) {
    if (remaining.isEmpty) {
      result.add([...current]);
      return;
    }
    
    for (int i = 0; i < remaining.length; i++) {
      current.add(remaining[i]);
      List<int> newRemaining = [...remaining];
      newRemaining.removeAt(i);
      backtrack(current, newRemaining);
      current.removeLast();
    }
  }
  
  backtrack([], arr);
  return result;
}
// For [1, 2, 3]: 3! = 6 permutations
// For [1, 2, 3, 4]: 4! = 24 permutations
// For [1, 2, 3, 4, 5, 6]: 6! = 720 permutations
// For 10 items: 10! = 3,628,800 permutations 💀

// Example 2: Traveling Salesman Problem (Brute Force)
// Try every possible route to visit n cities
// Total routes: n!
```

#### Time Visualization
```
n = 5:   ▓ 120 operations
n = 6:   ▓ 720 operations
n = 7:   ▓ 5,040 operations
n = 8:   ▓ 40,320 operations
n = 10:  ▓ 3,628,800 operations
n = 12:  ▓ 479,001,600 operations 🔥
n = 15:  ▓ 1,307,674,368,000 operations 💀
```

#### Interview Answer
> "We generate every possible ordering. For n items, that's n × (n-1) × (n-2) × ... × 1 = **O(n!)**. This is the slowest complexity—almost never acceptable."

---

## Common Mistakes {#mistakes}

### ❌ Mistake 1: Counting Operations Wrong

**WRONG:**
```dart
for (int i = 0; i < n; i++) {
  for (int j = 0; j < n; j++) {
    for (int k = 0; k < n; k++) {
      operation();  // Thinking this is O(n³)
    }
  }
}
```
**CORRECT:** This IS O(n³) - three nested loops!

### ❌ Mistake 2: Ignoring Constants

**WRONG:**
```dart
for (int i = 0; i < n; i++) {      // Called O(n)
  operation1();
  operation2();
  operation3();
  operation4();
  operation5();
}
// Thinking: "5 operations per iteration, so O(5n)!"
```
**CORRECT:** We drop constants, so it's **O(n)** not O(5n).

### ❌ Mistake 3: Confusing Different Loops

**WRONG:**
```dart
for (int i = 0; i < n; i++) { }      // O(n)
for (int j = 0; j < m; j++) { }      // O(m)
// Thinking: O(n) + O(m) = O(n + m) but saying O(n)
```
**CORRECT:** If both loops depend on different inputs, it's **O(n + m)** not O(n).

### ❌ Mistake 4: Confusing Recursion Depth with Calls

**WRONG:**
```dart
void recurse(int n) {
  if (n == 0) return;
  print(n);
  recurse(n - 1);
}
// Thinking: "I call recurse once, so it's O(n)?"
```
**CORRECT:** It IS O(n) because you make n calls total. But this is right by accident—your reasoning is unclear.

### ❌ Mistake 5: Assuming String Operations are O(1)

**WRONG:**
```dart
String result = "";
for (int i = 0; i < n; i++) {
  result = result + "x";  // String concatenation
}
// Thinking: "Just adding to a string, O(n)?"
```
**CORRECT:** String concatenation creates a new string each time.
- Operation 1: Create string of length 1
- Operation 2: Create string of length 2 (copy all 1 + new 1)
- Operation 3: Create string of length 3 (copy all 2 + new 1)
- ...
- Total: 1 + 2 + 3 + ... + n = **O(n²)**

**FIX:** Use List and join:
```dart
List<String> result = [];
for (int i = 0; i < n; i++) {
  result.add("x");  // O(1) each
}
String final = result.join("");  // O(n) once
// Now it's O(n) total!
```

---

## Real-World Analogies {#analogies}

### O(1) - Direct Dictionary Lookup
📖 You have a phone book. You look up "Alice" and instantly find her phone number.
- No matter if there are 100 or 1 million names, finding one is instant.

### O(log n) - Binary Search (Phonebook Style)
📖 You search for a name in a phonebook:
- Open to the middle
- "Is it before or after this name?"
- Repeat with the half you chose
- Each step eliminates half the remaining names

### O(n) - Reading a List
📋 Reading every name on a to-do list, top to bottom.
- 10 tasks = 10 reads
- 100 tasks = 100 reads

### O(n²) - Comparing Everyone to Everyone
👥 In a classroom of students, you want to find if any two have the same birthday:
- Student 1 compares with: 2, 3, 4, ... (n comparisons)
- Student 2 compares with: 3, 4, 5, ... (n-1 comparisons)
- ...
- Total: n × n = n²

### O(n log n) - Merge Sort
📚 Sorting books:
1. Divide books in half repeatedly (log n steps)
2. Merge sorted halves back together (n work)
- Like organizing papers by dividing into piles, then merging piles

### O(2ⁿ) - Tree Growth
🌳 Your family tree:
- You: 1
- Parents: 2
- Grandparents: 4
- Great-grandparents: 8
- ... doubles each generation

### O(n!) - Seating Arrangements
🪑 Arranging guests at a dinner table:
- 5 guests: 120 different arrangements
- 10 guests: 3.6 million arrangements
- 15 guests: 1.3 trillion arrangements

---

## Practice Problems {#practice}

### Problem 1: Find the Time Complexity

```dart
void problem1(List<int> arr) {
  for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr.length; j++) {
      print(arr[i] + arr[j]);
    }
  }
}
```

<details>
<summary>🔍 Click to see answer</summary>

**Time Complexity: O(n²)**

**Explanation:** Two nested loops, each running n times. Total operations: n × n = n²

**Interview Answer:** "We have two nested loops, each traversing the entire array. That's O(n) × O(n) = **O(n²)**."

</details>

---

### Problem 2: Find the Time Complexity

```dart
void problem2(int n) {
  for (int i = 0; i < n; i++) {
    print(i);
  }
  
  for (int j = 0; j < n; j++) {
    print(j);
  }
}
```

<details>
<summary>🔍 Click to see answer</summary>

**Time Complexity: O(n)**

**Explanation:** Two separate loops, each O(n). Total: O(n) + O(n) = O(2n). Constants are dropped, so **O(n)**.

**Interview Answer:** "The two loops are separate, not nested. First loop: O(n), Second loop: O(n). Total: O(n + n) = O(2n), which simplifies to **O(n)**."

</details>

---

### Problem 3: Find the Time Complexity

```dart
int search(List<int> arr, int target) {
  int left = 0, right = arr.length - 1;
  
  while (left <= right) {
    int mid = (left + right) ~/ 2;
    
    if (arr[mid] == target) return mid;
    else if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  
  return -1;
}
```

<details>
<summary>🔍 Click to see answer</summary>

**Time Complexity: O(log n)**

**Explanation:** The search space is halved in each iteration:
- Start: [1, 2, 3, 4, 5, 6, 7, 8] (8 elements)
- Step 1: Check middle, search [1-4] or [5-8] (4 elements)
- Step 2: Check middle, search half again (2 elements)
- Step 3: Check middle, (1 element)

For n items, we need about log₂(n) steps.

**Interview Answer:** "This is binary search. Each iteration eliminates half of the remaining elements. For n elements, we need ~log₂(n) steps. That's **O(log n)**—super efficient!"

</details>

---

### Problem 4: Find the Time Complexity

```dart
List<int> merge(List<int> left, List<int> right) {
  List<int> result = [];
  int i = 0, j = 0;
  
  while (i < left.length && j < right.length) {
    if (left[i] <= right[j]) {
      result.add(left[i++]);
    } else {
      result.add(right[j++]);
    }
  }
  
  result.addAll(left.sublist(i));
  result.addAll(right.sublist(j));
  
  return result;
}
```

<details>
<summary>🔍 Click to see answer</summary>

**Time Complexity: O(n)**

**Explanation:** We go through each element of left and right arrays exactly once. Total elements processed: left.length + right.length = n.

**Interview Answer:** "We merge two sorted arrays by comparing elements one at a time. Each element is touched exactly once. If left has m elements and right has n elements, we touch m + n = **O(n)** elements total."

</details>

---

### Problem 5: Space Complexity Challenge

```dart
List<int> buildFrequency(String str) {
  List<int> freq = List.filled(26, 0);  // a-z
  
  for (int i = 0; i < str.length; i++) {
    int charIndex = str[i].codeUnitAt(0) - 'a'.codeUnitAt(0);
    freq[charIndex]++;
  }
  
  return freq;
}
```

<details>
<summary>🔍 Click to see answer</summary>

**Time Complexity: O(n)**
- Loop runs n times (str.length)

**Space Complexity: O(1)**
- We create a list of size 26 (for 26 letters)
- 26 is a constant, not dependent on input size
- We ignore constants in Big O

**Interview Answer:** 
- Time: "The string is traversed once: **O(n)**."
- Space: "We create a fixed-size array of 26 elements (for a-z). The size doesn't grow with input, so it's **O(1)**."

</details>

---

## Interview Answers {#interviews}

### Standard Interview Scenario 1

**Interviewer:** "What's the time complexity of this code?"

```dart
for (int i = 0; i < n; i++) {
  for (int j = 0; j < n; j++) {
    operation();
  }
}
```

**Bad Answer:** "Umm... O(n)?"

**Good Answer:** "This has two nested loops. The outer loop runs n times, and for each iteration, the inner loop runs n times. So we have **O(n²)**. For 1,000 items, that's 1,000,000 operations."

**Expert Answer:** "Time complexity is **O(n²)**. The outer loop runs n times, and the inner loop also runs n times for each outer iteration, resulting in n × n operations. Space complexity is **O(1)** because we're not using any extra data structures. This becomes slow for large inputs—for n=10,000, we get 100 million operations."

---

### Standard Interview Scenario 2

**Interviewer:** "Can you improve this to O(n)?"

```dart
bool hasDuplicate(List<int> arr) {
  for (int i = 0; i < arr.length; i++) {
    for (int j = i + 1; j < arr.length; j++) {
      if (arr[i] == arr[j]) {
        return true;
      }
    }
  }
  return false;
}
```

**Bad Answer:** "I don't think it can be done."

**Good Answer:** "Yes! Instead of comparing every element with every other element (O(n²)), I can use a Set to track seen elements. As I iterate through the array once, I check if the current element is already in the Set."

```dart
bool hasDuplicate(List<int> arr) {
  Set<int> seen = {};
  
  for (int i = 0; i < arr.length; i++) {
    if (seen.contains(arr[i])) {
      return true;
    }
    seen.add(arr[i]);
  }
  
  return false;
}
// Time: O(n) - One pass through array
// Space: O(n) - Set stores up to n elements
```

**Expert Answer:** "The original is O(n²), which is inefficient. I'd use a Set:
- **Time:** O(n) — traverse array once
- **Space:** O(n) — Set stores up to n elements
- **Tradeoff:** We use extra memory to save time
- **For large inputs:** If n=1,000,000, the first solution does 1 trillion operations; the Set solution does just 1 million."

---

### Standard Interview Scenario 3

**Interviewer:** "What about space complexity?"

**Bad Answer:** "I didn't think about that."

**Good Answer:** "Let me analyze both:
- **Time:** O(n log n)
- **Space:** O(n) because we're creating a new array"

**Expert Answer:** "Great question!
- **Time:** O(n log n) — We use merge sort, which divides (log n) and merges (n)
- **Space:** O(n) — We create temporary arrays during merging. Even though we later delete them, at peak we use O(n) extra space.
- **Tradeoff:** This is worth it because we get O(n log n) time instead of O(n²)"

---

## Quick Reference {#reference}

### Time Complexity Cheat Sheet

| Complexity | Name | Operations for n=1000 | When to Use | Example |
|----------|------|------|---------|----------|
| O(1) | Constant | 1 | Direct access, hash lookup | Dictionary lookup |
| O(log n) | Logarithmic | ~10 | Divide & conquer, binary search | Binary search |
| O(n) | Linear | 1,000 | Traversal | Find max, count items |
| O(n log n) | Linearithmic | ~10,000 | Efficient sorting | Merge sort |
| O(n²) | Quadratic | 1,000,000 | ⚠️ Slow but possible | Bubble sort, nested loops |
| O(n³) | Cubic | 1,000,000,000 | 🚫 Very slow | Matrix multiplication |
| O(2ⁿ) | Exponential | 10³⁰ | 💀 Unacceptable | Fibonacci, subsets |
| O(n!) | Factorial | 10^2000 | 🔥 Never use | Permutations |

### Space Complexity Cheat Sheet

| Complexity | Example | Interview Phrase |
|----------|---------|---------|
| O(1) | Few variables | "Constant extra space" |
| O(log n) | Recursion depth | "Space for call stack" |
| O(n) | Extra array or Set | "Proportional to input size" |
| O(n²) | 2D array output | "Output size dominates" |

### Decision Tree

**To find time complexity:**

```
Does the algorithm do the same work regardless of input?
├─ YES → O(1)
└─ NO → Does it eliminate half the problem each time?
    ├─ YES → O(log n)
    └─ NO → Does it touch each element once?
        ├─ YES → O(n)
        └─ NO → Does it have nested loops?
            ├─ YES → O(n²) or higher
            └─ NO → Does it recurse without memoization?
                ├─ YES → O(2ⁿ) or O(n!)
                └─ NO → Look at the algorithm more carefully
```

### Golden Rules

#### For Time Complexity
1. **Count iterations** — How many times do we execute operations?
2. **Check for recursion** — Does problem size grow or shrink?
3. **Look for loops** — Nested loops multiply complexity
4. **Drop constants** — O(5n) = O(n), O(n + 100) = O(n)
5. **Keep dominant term** — O(n² + n) = O(n²)

#### For Space Complexity
1. **Count extra memory** — Not including input size
2. **Check for recursion depth** — Call stack uses space
3. **Data structures** — Set, Map, List all use extra space
4. **Drop constants** — O(26) (alphabet) = O(1)

---

## Summary Table: Every Complexity Explained

| Big O | Name | Growth Pattern | When You See It | Real Impact (n=1M) |
|-------|------|---|---|---|
| **O(1)** | Constant | Same work always | Direct access, hash lookup | <1ms ⚡ |
| **O(log n)** | Logarithmic | Cuts in half | Binary search | ~20ms 🚀 |
| **O(n)** | Linear | Straight line | Loop through array | ~1 second ✅ |
| **O(n log n)** | Linearithmic | Fast growth | Efficient sorting | ~20 seconds 📊 |
| **O(n²)** | Quadratic | Nested loops | Bubble sort | **11 days** 🐌 |
| **O(n³)** | Cubic | Triple nested loops | Matrix multiplication | **31 years** 😱 |
| **O(2ⁿ)** | Exponential | Doubles each step | Fibonacci, subsets | **300K years** 💀 |
| **O(n!)** | Factorial | Massive growth | Permutations | **Infinity** 🔥 |

---

## Interview Checklist

Before your interview, make sure you can:

- [ ] **Explain Big O** in simple terms without using formulas
- [ ] **Identify O(1), O(n), O(n²)** by looking at code
- [ ] **Explain why constants don't matter** with an example
- [ ] **Optimize an O(n²) algorithm** to O(n)
- [ ] **Discuss time vs space tradeoffs**
- [ ] **Calculate complexity** for nested and separate loops
- [ ] **Recognize patterns** (recursion → exponential, nested loops → polynomial)
- [ ] **Explain recursion depth** and how it affects space
- [ ] **Give real-world analogies** for each complexity class
- [ ] **Mention both time AND space complexity** in answers

---

## Final Tips

✅ **DO:**
- Always consider both time AND space complexity
- Start with a simple solution, then optimize
- Test your assumptions with examples
- Communicate clearly why you chose a complexity
- Use data structures strategically (Set, Map, etc.)

❌ **DON'T:**
- Overoptimize early (clarity matters first)
- Forget about space complexity
- Confuse nested loops with separate loops
- Assume string operations are O(1)
- Use O(2ⁿ) algorithms for anything larger than n=20

---

**Good luck with your interviews! You've got this! 🚀**
