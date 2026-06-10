# Coding Round Questions - Dart Solutions With Easy Explanations

Use this file to prepare for coding interviews. For every question, remember this speaking format:

1. **Understand** the problem with examples
2. **Visualize** how your approach works step-by-step
3. **Code** it with clear logic
4. **Optimize** if needed
5. **Discuss** time and space complexity

---

## Table of Contents
1. [Arrays](#1-arrays)
2. [Strings](#2-strings)
3. [Linked Lists](#3-linked-lists)
4. [Trees & Binary Search Trees](#4-trees--binary-search-trees)
5. [Pattern Questions](#5-pattern-questions)
6. [Number Problems](#6-number-problems)
7. [Recursion](#7-recursion)
8. [Dynamic Programming](#8-dynamic-programming)
9. [Stack & Queue](#9-stack--queue)
10. [Searching & Sorting](#10-searching--sorting)
11. [Common Interview Problems](#11-common-interview-problems)

---

## 1. Arrays

### 1.1 Find the second largest element in an array

**Problem Statement:**
Given an array of integers, find the second largest element. Return `null` if array has fewer than 2 elements.

**Examples:**
```
Input: [10, 20, 50, 30, 40]
Output: 40 (largest is 50, second largest is 40)

Input: [5, 5, 5, 5]
Output: 5

Input: [1]
Output: null
```

**Approach:**
- Use two variables: `largest` and `second`
- Scan array once
- When you find a new largest, push the old largest to second
- Update largest with the new value

**Visual Walkthrough:**
```
Array: [10, 20, 50, 30, 40]

Step 1: num=10  → largest=10, second=null
Step 2: num=20  → 20 > 10 → second=10, largest=20
Step 3: num=50  → 50 > 20 → second=20, largest=50
Step 4: num=30  → 30 < 50 but 30 > 20 → second=30
Step 5: num=40  → 40 < 50 but 40 > 30 → second=40

Output: 40 ✓
```

**Dart Code:**
```dart
int? secondLargest(List<int> nums) {
  if (nums.length < 2) return null;

  int? largest;
  int? second;

  for (final num in nums) {
    if (largest == null || num > largest) {
      second = largest;      // Old largest becomes second
      largest = num;         // Update to new largest
    } else if (num != largest && (second == null || num > second)) {
      // Only update second if num is different from largest
      // and it's bigger than current second
      second = num;
    }
  }

  return second;
}
```

**Key Points to Explain in Interview:**
- "I avoid duplicates by checking `num != largest`"
- "Single pass means O(n) time"
- "Only 2 variables needed, so O(1) space"

**Complexity:** Time `O(n)`, Space `O(1)`

**Practice Questions:**
1. Find the **third largest** element
2. Find the **largest and second largest** (return both)
3. Find the second largest **without sorting**
4. Handle **negative numbers** and **duplicates**

**Common Mistakes:**
- ❌ Sorting the array (wastes time: O(n log n))
- ❌ Not handling case where all elements are same
- ❌ Using `>=` instead of `>` (causes duplicates issue)

---

### 1.2 Check if the array is sorted

**Problem Statement:**
Determine if an array is sorted in ascending order.

**Examples:**
```
Input: [1, 2, 3, 4, 5]
Output: true

Input: [1, 3, 2, 4]
Output: false (3 comes before 2, breaks order)

Input: [5, 5, 5]
Output: true (equal elements are allowed)
```

**Approach:**
- Compare each element with its next element
- If any element is greater than next, return false immediately
- If loop completes, return true

**Visual Walkthrough:**
```
Array: [1, 2, 4, 3, 5]

Compare: 1 <= 2? YES
Compare: 2 <= 4? YES
Compare: 4 <= 3? NO → Return false ✓
```

**Dart Code:**
```dart
bool isSorted(List<int> nums) {
  for (int i = 0; i < nums.length - 1; i++) {
    if (nums[i] > nums[i + 1]) {
      return false;  // Found unsorted pair, return immediately
    }
  }
  return true;  // All pairs are in order
}
```

**Complexity:** Time `O(n)`, Space `O(1)`

**Practice Questions:**
1. Check if sorted in **descending** order
2. Check if sorted in **ascending OR descending** order
3. Check if sorted **in any rotated state**
4. **Count inversions** (pairs where larger comes before smaller)

**Common Mistakes:**
- ❌ Loop goes to `nums.length` (causes index out of bounds)
- ❌ Using `<` instead of `<=` (misses duplicate check)

---

### 1.3 Remove duplicates from a sorted array

**Problem Statement:**
Remove duplicate elements from a sorted array **in-place**. Return the count of unique elements.

**Example:**
```
Input: [1, 1, 2, 2, 2, 3, 4, 4]
Output: 5
Modified array: [1, 2, 3, 4, _, _, _, _] (first 5 elements are unique)

The underscore represents "don't care" values
```

**Approach (Two Pointer):**
- `read` pointer scans the entire array
- `write` pointer marks position for next unique element
- Only write when we find a different value

**Visual Walkthrough:**
```
Array: [1, 1, 2, 2, 3]

Initial: write=1 (start from position 1)

i=1, read=1: nums[1]=1, nums[0]=1 → Same, skip
i=2, read=2: nums[2]=2, nums[0]=1 → Different! Write at write=1
            write++, so write=2
i=3, read=3: nums[3]=2, nums[1]=2 → Same, skip
i=4, read=4: nums[4]=3, nums[1]=2 → Different! Write at write=2
            write++, so write=3

Return: 3
Array: [1, 2, 3, _, _]
```

**Dart Code:**
```dart
int removeDuplicates(List<int> nums) {
  if (nums.isEmpty) return 0;

  int write = 1;  // Position to write next unique element
  
  for (int read = 1; read < nums.length; read++) {
    // Compare current element with last written element
    if (nums[read] != nums[write - 1]) {
      nums[write] = nums[read];  // Write unique element
      write++;
    }
  }
  
  return write;  // Count of unique elements
}
```

**Complexity:** Time `O(n)`, Space `O(1)` (in-place)

**Practice Questions:**
1. Remove duplicates allowing **at most 2 occurrences**
2. Remove duplicates and **return the modified array**
3. Remove duplicates **without modifying** the array (return new list)
4. **Count the number of duplicates**

**Common Mistakes:**
- ❌ Creating a new array (violates in-place requirement)
- ❌ Using `nums[read] != nums[read-1]` (doesn't work correctly)
- ❌ Not returning the count

---

### 1.4 Move all zeros to the end

**Problem Statement:**
Move all zeros to the end of array while keeping order of non-zero elements. Modify in-place.

**Example:**
```
Input: [0, 1, 0, 3, 12]
Output: [1, 3, 12, 0, 0]

Input: [0, 0, 1]
Output: [1, 0, 0]
```

**Approach:**
- Maintain a `write` pointer for next non-zero position
- Scan entire array
- Write non-zero elements sequentially
- Fill remaining positions with zeros

**Visual Walkthrough:**
```
Array: [0, 1, 0, 3, 12]

Initial: write = 0

i=0: nums[0]=0 → Zero, skip
i=1: nums[1]=1 → Non-zero! Write at position 0
     nums[0]=1, write=1
i=2: nums[2]=0 → Zero, skip
i=3: nums[3]=3 → Non-zero! Write at position 1
     nums[1]=3, write=2
i=4: nums[4]=12 → Non-zero! Write at position 2
     nums[2]=12, write=3

Fill rest with zeros:
nums[3]=0, nums[4]=0

Result: [1, 3, 12, 0, 0] ✓
```

**Dart Code:**
```dart
void moveZerosToEnd(List<int> nums) {
  int write = 0;

  // Write all non-zero elements
  for (final num in nums) {
    if (num != 0) {
      nums[write] = num;
      write++;
    }
  }

  // Fill remaining positions with zeros
  while (write < nums.length) {
    nums[write] = 0;
    write++;
  }
}
```

**Complexity:** Time `O(n)`, Space `O(1)` (in-place)

**Practice Questions:**
1. Move all **negative numbers** to start
2. Move all **zeros** to middle (keep relative order)
3. **Count total zeros** while moving them
4. Arrange so **odd numbers before even** numbers

---

### 1.5 Find the missing number in an array of 1 to N

**Problem Statement:**
Array contains numbers from 1 to N, but one is missing. Find it.

**Example:**
```
Input: [1, 2, 4, 5], N=5
Output: 3 (missing number)

Input: [1, 2, 3], N=4
Output: 4
```

**Approach (Math):**
- Sum of 1 to N = `n * (n + 1) / 2`
- Sum of actual array = actual sum
- Missing = Expected - Actual

**Visual Walkthrough:**
```
N=5, Array=[1, 2, 4, 5]

Expected sum = 5 * 6 / 2 = 15
Actual sum = 1 + 2 + 4 + 5 = 12
Missing = 15 - 12 = 3 ✓
```

**Dart Code:**
```dart
int missingNumber(List<int> nums, int n) {
  // Calculate expected sum: 1 + 2 + 3 + ... + n
  final expected = n * (n + 1) ~/ 2;
  
  // Calculate actual sum
  final actual = nums.fold<int>(0, (sum, value) => sum + value);
  
  return expected - actual;
}
```

**Complexity:** Time `O(n)`, Space `O(1)`

**Practice Questions:**
1. Find **missing numbers** (plural, multiple missing)
2. Array contains 0 to N, find missing
3. Find missing with **duplicates** allowed

---

### 1.6 Kadane's Algorithm (Maximum Subarray Sum)

**Problem Statement:**
Find the contiguous subarray with largest sum.

**Example:**
```
Input: [-2, 1, -3, 4, -1, 2, 1, -5, 4]
Output: 6 (subarray [4, -1, 2, 1])

Input: [5, 4, -1, 7, 8]
Output: 23 (entire array)

Input: [-1, -2, -3]
Output: -1 (single largest element)
```

**Approach:**
- At each position, decide: start new subarray or extend previous?
- Keep track of best sum seen so far

**Visual Walkthrough:**
```
Array: [-2, 1, -3, 4, -1, 2, 1]

i=0: num=-2
  current = -2
  best = -2

i=1: num=1
  Option 1: Start new: 1
  Option 2: Extend previous: -2 + 1 = -1
  current = 1 (start new is better)
  best = 1

i=3: num=4
  current = 4 (start new)
  best = 4

i=6: num=1
  current = 5 + 1 = 6
  best = 6 ✓
```

**Dart Code:**
```dart
int maxSubArray(List<int> nums) {
  int current = nums[0];  // Best sum ending at current index
  int best = nums[0];     // Best sum seen so far

  for (int i = 1; i < nums.length; i++) {
    // Either start fresh or extend previous sum
    current = max(nums[i], current + nums[i]);
    best = max(best, current);
  }

  return best;
}

// Helper function
int max(int a, int b) => a > b ? a : b;
```

**Complexity:** Time `O(n)`, Space `O(1)`

**Practice Questions:**
1. Find **minimum subarray sum**
2. Find **maximum sum circular subarray** (wraps around)
3. Return the **actual subarray**, not just sum

---

## 2. Strings

### 2.1 Reverse a string

**Problem Statement:**
Reverse the characters in a string.

**Example:**
```
Input: "hello"
Output: "olleh"

Input: "a"
Output: "a"
```

**Dart Code:**
```dart
String reverseString(String s) {
  return s.split('').reversed.join();
}
```

**Complexity:** Time `O(n)`, Space `O(n)`

---

### 2.2 Check if a string is a palindrome

**Problem Statement:**
Determine if string reads the same forwards and backwards.

**Example:**
```
Input: "racecar"
Output: true

Input: "hello"
Output: false
```

**Approach (Two Pointer):**
- Start with pointers at both ends
- Compare characters moving inward
- If mismatch found, return false

**Dart Code:**
```dart
bool isPalindrome(String s) {
  int left = 0;
  int right = s.length - 1;

  while (left < right) {
    if (s[left] != s[right]) return false;
    left++;
    right--;
  }
  return true;
}
```

**Complexity:** Time `O(n)`, Space `O(1)`

---

### 2.3 Check if two strings are anagrams

**Problem Statement:**
Anagrams have same characters with same frequencies, just rearranged.

**Example:**
```
Input: "listen", "silent"
Output: true

Input: "hello", "world"
Output: false
```

**Dart Code:**
```dart
bool areAnagrams(String a, String b) {
  if (a.length != b.length) return false;

  final count = <String, int>{};
  
  for (final ch in a.split('')) {
    count[ch] = (count[ch] ?? 0) + 1;
  }
  
  for (final ch in b.split('')) {
    if (!count.containsKey(ch)) return false;
    count[ch] = count[ch]! - 1;
    if (count[ch] == 0) count.remove(ch);
  }
  
  return count.isEmpty;
}
```

**Complexity:** Time `O(n)`, Space `O(k)`

---

### 2.4 Find the first non-repeating character

**Problem Statement:**
Find the first character that appears exactly once.

**Example:**
```
Input: "leetcode"
Output: "l"

Input: "aab"
Output: null
```

**Dart Code:**
```dart
String? firstNonRepeating(String s) {
  final count = <String, int>{};

  for (final ch in s.split('')) {
    count[ch] = (count[ch] ?? 0) + 1;
  }

  for (final ch in s.split('')) {
    if (count[ch] == 1) return ch;
  }

  return null;
}
```

**Complexity:** Time `O(n)`, Space `O(k)`

---

### 2.5 Remove duplicate characters from a string

**Problem Statement:**
Remove duplicate characters keeping only first occurrence. Maintain order.

**Dart Code:**
```dart
String removeDuplicateChars(String s) {
  final seen = <String>{};
  final result = StringBuffer();

  for (final ch in s.split('')) {
    if (seen.add(ch)) {
      result.write(ch);
    }
  }

  return result.toString();
}
```

**Complexity:** Time `O(n)`, Space `O(k)`

---

## 3. Linked Lists

### 3.1 Reverse a Linked List

**Problem Statement:**
Reverse the direction of links in a linked list.

**Example:**
```
Input:  1 → 2 → 3 → null
Output: 3 → 2 → 1 → null
```

**Approach:**
- Keep three pointers: previous, current, next
- Reverse links one by one
- Move forward through list

**Visual Walkthrough:**
```
Initial: 1 → 2 → 3 → null

Step 1: prev=null, curr=1, next=2
        Save next: 2
        curr.next = null
        Move: prev=1, curr=2
        Result: null ← 1  2 → 3 → null

Step 2: prev=1, curr=2, next=3
        Save next: 3
        curr.next = 1
        Move: prev=2, curr=3
        Result: 1 ← 2  3 → null

Step 3: prev=2, curr=3, next=null
        Save next: null
        curr.next = 2
        Move: prev=3, curr=null
        Result: null ← 1 ← 2 ← 3

Return: 3 ✓
```

**Dart Code:**
```dart
class Node {
  int value;
  Node? next;
  
  Node(this.value, [this.next]);
}

// Iterative approach
Node? reverseLinkedList(Node? head) {
  Node? prev = null;
  Node? current = head;
  
  while (current != null) {
    final next = current.next;    // Save next node
    current.next = prev;           // Reverse the link
    prev = current;                // Move prev forward
    current = next;                // Move current forward
  }
  
  return prev;  // New head
}
```

**Complexity:** Time `O(n)`, Space `O(1)`

**Practice Questions:**
1. Reverse **part of the list** (from position m to n)
2. Reverse **in groups of k**
3. **Recursive approach** to reverse
4. Find **kth node from end**

---

### 3.2 Detect Cycle in Linked List

**Problem Statement:**
Determine if linked list contains a cycle.

**Example:**
```
1 → 2 → 3 → 4
        ↑   ↓
        └───┘  (cycle present)
```

**Approach (Floyd's Cycle Detection):**
- Use two pointers: slow (moves 1 step), fast (moves 2 steps)
- If they meet, cycle exists
- If fast reaches null, no cycle

**Dart Code:**
```dart
bool hasCycle(Node? head) {
  Node? slow = head;
  Node? fast = head;
  
  while (fast != null && fast.next != null) {
    slow = slow!.next;           // Move 1 step
    fast = fast.next!.next;      // Move 2 steps
    
    if (slow == fast) {
      return true;  // They met, cycle found
    }
  }
  
  return false;  // fast reached null, no cycle
}
```

**Complexity:** Time `O(n)`, Space `O(1)`

---

### 3.3 Find Middle of Linked List

**Problem Statement:**
Find the middle node of linked list.

**Example:**
```
Input:  1 → 2 → 3 → 4 → 5
Output: 3 (middle node)

Input:  1 → 2 → 3 → 4
Output: 3 (or 4, depending on definition)
```

**Dart Code:**
```dart
Node? findMiddle(Node? head) {
  Node? slow = head;
  Node? fast = head;
  
  // When fast reaches end, slow is at middle
  while (fast != null && fast.next != null) {
    slow = slow!.next;
    fast = fast.next!.next;
  }
  
  return slow;
}
```

**Complexity:** Time `O(n)`, Space `O(1)`

---

## 4. Trees & Binary Search Trees

### 4.1 Inorder Traversal of Binary Tree

**Problem Statement:**
Visit nodes in order: Left, Root, Right

**Example:**
```
    1
   / \
  2   3
  
Output: [2, 1, 3]
```

**Visual Walkthrough:**
```
Tree:     1
         / \
        2   3

Traversal:
1. Visit left of 1 → node 2
2. Visit left of 2 → null
3. Add 2 to result
4. Visit right of 2 → null
5. Add 1 to result
6. Visit right of 1 → node 3
7. Visit left of 3 → null
8. Add 3 to result
9. Visit right of 3 → null

Result: [2, 1, 3] ✓
```

**Dart Code:**
```dart
class TreeNode {
  int value;
  TreeNode? left;
  TreeNode? right;
  
  TreeNode(this.value, [this.left, this.right]);
}

List<int> inorderTraversal(TreeNode? root) {
  final result = <int>[];
  
  void traverse(TreeNode? node) {
    if (node == null) return;
    
    traverse(node.left);       // Go left
    result.add(node.value);    // Visit root
    traverse(node.right);      // Go right
  }
  
  traverse(root);
  return result;
}

// Iterative approach using stack
List<int> inorderTraversalIterative(TreeNode? root) {
  final result = <int>[];
  final stack = <TreeNode>[];
  TreeNode? current = root;
  
  while (current != null || stack.isNotEmpty) {
    while (current != null) {
      stack.add(current);
      current = current.left;
    }
    
    current = stack.removeLast();
    result.add(current.value);
    current = current.right;
  }
  
  return result;
}
```

**Complexity:** Time `O(n)`, Space `O(h)`

---

### 4.2 Level Order Traversal (BFS)

**Problem Statement:**
Visit nodes level by level from top to bottom.

**Example:**
```
    1
   / \
  2   3
 / \
4   5

Output: [[1], [2, 3], [4, 5]]
```

**Dart Code:**
```dart
List<List<int>> levelOrderTraversal(TreeNode? root) {
  final result = <List<int>>[];
  if (root == null) return result;
  
  final queue = <TreeNode>[root];
  
  while (queue.isNotEmpty) {
    final levelSize = queue.length;
    final currentLevel = <int>[];
    
    for (int i = 0; i < levelSize; i++) {
      final node = queue.removeAt(0);
      currentLevel.add(node.value);
      
      if (node.left != null) queue.add(node.left!);
      if (node.right != null) queue.add(node.right!);
    }
    
    result.add(currentLevel);
  }
  
  return result;
}
```

**Complexity:** Time `O(n)`, Space `O(w)` where w is max width

---

## 5. Pattern Questions

### 5.1 Print diamond pattern

**Pattern:**
```
    *
   * *
  * * *
 * * * *
* * * * *
 * * * *
  * * *
   * *
    *
```

**Dart Code:**
```dart
void diamond(int n) {
  // Upper half
  for (int i = 1; i <= n; i++) {
    print(List.filled(n - i, ' ').join() + List.filled(i, '* ').join());
  }
  // Lower half
  for (int i = n - 1; i >= 1; i--) {
    print(List.filled(n - i, ' ').join() + List.filled(i, '* ').join());
  }
}
```

---

## 6. Number Problems

### 6.1 Check if a number is prime

**Problem Statement:**
Determine if number is divisible only by 1 and itself.

**Example:**
```
Input: 17
Output: true

Input: 15
Output: false
```

**Approach:**
- Check divisibility up to √n

**Dart Code:**
```dart
bool isPrime(int n) {
  if (n <= 1) return false;
  for (int i = 2; i * i <= n; i++) {
    if (n % i == 0) return false;
  }
  return true;
}
```

**Complexity:** Time `O(√n)`, Space `O(1)`

---

### 6.2 Find GCD and LCM

**Problem Statement:**
- **GCD:** Largest number that divides both
- **LCM:** Smallest number divisible by both

**Example:**
```
GCD(12, 8) = 4
LCM(12, 8) = 24
```

**Dart Code:**
```dart
int gcd(int a, int b) {
  while (b != 0) {
    final temp = b;
    b = a % b;
    a = temp;
  }
  return a.abs();
}

int lcm(int a, int b) {
  return (a * b).abs() ~/ gcd(a, b);
}
```

**Complexity:** Time `O(log(min(a, b)))`, Space `O(1)`

---

### 6.3 Fibonacci Series

**Problem Statement:**
Generate first N Fibonacci numbers.

**Example:**
```
Input: 5
Output: [0, 1, 1, 2, 3]
```

**Dart Code:**
```dart
List<int> fibonacci(int n) {
  if (n <= 0) return [];
  if (n == 1) return [0];
  
  final result = [0, 1];
  for (int i = 2; i < n; i++) {
    result.add(result[i - 1] + result[i - 2]);
  }
  return result;
}
```

**Complexity:** Time `O(n)`, Space `O(n)`

---

## 7. Recursion

### 7.1 Factorial of a number

**Problem:** Calculate n! = n × (n-1) × ... × 1

**Dart Code:**
```dart
int factorial(int n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}
```

**Complexity:** Time `O(n)`, Space `O(n)` (call stack)

---

### 7.2 Power of a number

**Problem:** Calculate x^n efficiently

**Dart Code:**
```dart
int power(int x, int n) {
  if (n == 0) return 1;
  
  if (n % 2 == 0) {
    final half = power(x, n ~/ 2);
    return half * half;
  } else {
    return x * power(x, n - 1);
  }
}
```

**Complexity:** Time `O(log n)`, Space `O(log n)`

---

## 8. Dynamic Programming

### 8.1 Fibonacci with Memoization

**Problem:** Calculate Nth Fibonacci with optimization

**Dart Code:**
```dart
int fibMemo(int n, Map<int, int> memo) {
  if (n <= 1) return n;
  if (memo.containsKey(n)) return memo[n]!;
  
  memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
  return memo[n]!;
}

// Usage:
void main() {
  final memo = <int, int>{};
  print(fibMemo(10, memo));  // Output: 55
}
```

**Complexity:** Time `O(n)`, Space `O(n)`

---

### 8.2 Coin Change Problem

**Problem:** Find minimum coins to make amount

**Example:**
```
Coins: [1, 2, 5]
Amount: 5
Output: 1 (one 5-coin)

Coins: [2]
Amount: 3
Output: -1 (impossible)
```

**Dart Code:**
```dart
int coinChange(List<int> coins, int amount) {
  final dp = List<int>.filled(amount + 1, amount + 1);
  dp[0] = 0;
  
  for (int i = 1; i <= amount; i++) {
    for (final coin in coins) {
      if (coin <= i) {
        dp[i] = min(dp[i], dp[i - coin] + 1);
      }
    }
  }
  
  return dp[amount] > amount ? -1 : dp[amount];
}

int min(int a, int b) => a < b ? a : b;
```

**Complexity:** Time `O(amount × coins)`, Space `O(amount)`

---

## 9. Stack & Queue

### 9.1 Implement Stack using array

**Dart Code:**
```dart
class Stack<T> {
  final List<T> _items = [];

  void push(T value) => _items.add(value);

  T? pop() => _items.isEmpty ? null : _items.removeLast();

  T? peek() => _items.isEmpty ? null : _items.last;

  bool get isEmpty => _items.isEmpty;
  
  int get size => _items.length;
}
```

---

### 9.2 Check for balanced parentheses

**Problem:** Verify all brackets are properly closed

**Example:**
```
Input: "({[]})"
Output: true

Input: "({[}])"
Output: false
```

**Dart Code:**
```dart
bool isBalanced(String s) {
  final stack = <String>[];
  final pairs = {')': '(', ']': '[', '}': '{'};

  for (final ch in s.split('')) {
    if (pairs.containsValue(ch)) {
      stack.add(ch);  // Opening bracket
    } else if (pairs.containsKey(ch)) {  // Closing bracket
      if (stack.isEmpty || stack.removeLast() != pairs[ch]) {
        return false;
      }
    }
  }

  return stack.isEmpty;
}
```

**Complexity:** Time `O(n)`, Space `O(n)`

---

## 10. Searching & Sorting

### 10.1 Binary Search

**Problem:** Find element in sorted array in O(log n)

**Example:**
```
Input: [1, 3, 5, 7, 9], target=5
Output: 2 (index)

Input: [1, 3, 5, 7, 9], target=6
Output: -1 (not found)
```

**Dart Code:**
```dart
int binarySearch(List<int> nums, int target) {
  int left = 0;
  int right = nums.length - 1;

  while (left <= right) {
    final mid = left + (right - left) ~/ 2;
    if (nums[mid] == target) return mid;
    if (nums[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }

  return -1;
}
```

**Complexity:** Time `O(log n)`, Space `O(1)`

---

### 10.2 Merge Sort

**Problem:** Sort array in O(n log n)

**Dart Code:**
```dart
List<int> mergeSort(List<int> nums) {
  if (nums.length <= 1) return nums;

  final mid = nums.length ~/ 2;
  final left = mergeSort(nums.sublist(0, mid));
  final right = mergeSort(nums.sublist(mid));

  return merge(left, right);
}

List<int> merge(List<int> left, List<int> right) {
  final result = <int>[];
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

**Complexity:** Time `O(n log n)`, Space `O(n)`

---

## 11. Common Interview Problems

### 11.1 Two Sum Problem

**Problem:** Find two numbers that add up to target

**Example:**
```
Input: [2, 7, 11, 15], target=9
Output: [0, 1]

Input: [3, 3], target=6
Output: [0, 1]
```

**Dart Code:**
```dart
List<int> twoSum(List<int> nums, int target) {
  final seen = <int, int>{};

  for (int i = 0; i < nums.length; i++) {
    final complement = target - nums[i];
    if (seen.containsKey(complement)) {
      return [seen[complement]!, i];
    }
    seen[nums[i]] = i;
  }

  return [];
}
```

**Complexity:** Time `O(n)`, Space `O(n)`

---

### 11.2 Find majority element (Boyer-Moore Voting)

**Problem:** Find element appearing more than n/2 times

**Example:**
```
Input: [3, 2, 3]
Output: 3

Input: [1]
Output: 1
```

**Dart Code:**
```dart
int? majorityElement(List<int> nums) {
  int? candidate;
  int count = 0;

  for (final num in nums) {
    if (count == 0) candidate = num;
    count += num == candidate ? 1 : -1;
  }

  final frequency = nums.where((x) => x == candidate).length;
  return frequency > nums.length ~/ 2 ? candidate : null;
}
```

**Complexity:** Time `O(n)`, Space `O(1)`

---

### 11.3 Trapping Rain Water

**Problem:** Calculate water trapped between elevation changes

**Dart Code:**
```dart
int trapRainWater(List<int> height) {
  int left = 0;
  int right = height.length - 1;
  int leftMax = 0;
  int rightMax = 0;
  int water = 0;

  while (left < right) {
    if (height[left] < height[right]) {
      if (height[left] >= leftMax) {
        leftMax = height[left];
      } else {
        water += leftMax - height[left];
      }
      left++;
    } else {
      if (height[right] >= rightMax) {
        rightMax = height[right];
      } else {
        water += rightMax - height[right];
      }
      right--;
    }
  }

  return water;
}
```

**Complexity:** Time `O(n)`, Space `O(1)`

---

### 11.4 Longest Substring Without Repeating Characters

**Problem:** Find longest substring with all unique characters

**Example:**
```
Input: "abcabcbb"
Output: 3 (substring "abc")

Input: "bbbbb"
Output: 1 (substring "b")

Input: "pwwkew"
Output: 3 (substring "wke")
```

**Dart Code:**
```dart
int lengthOfLongestSubstring(String s) {
  final charIndex = <String, int>{};
  int maxLength = 0;
  int left = 0;

  for (int right = 0; right < s.length; right++) {
    final ch = s[right];
    
    if (charIndex.containsKey(ch) && charIndex[ch]! >= left) {
      left = charIndex[ch]! + 1;
    }
    
    charIndex[ch] = right;
    maxLength = max(maxLength, right - left + 1);
  }

  return maxLength;
}

int max(int a, int b) => a > b ? a : b;
```

**Complexity:** Time `O(n)`, Space `O(min(n, alphabet))`

---

## Interview Preparation Checklist

- [ ] Understand the problem statement fully
- [ ] Ask clarification questions (edge cases, constraints)
- [ ] Draw examples before coding
- [ ] Explain your approach before writing code
- [ ] Code with clear variable names
- [ ] Test with examples (including edge cases)
- [ ] Analyze time and space complexity
- [ ] Discuss optimizations if possible

## Time Complexity Quick Reference

| Operation | Complexity |
|-----------|-----------|
| Array access | O(1) |
| Array search | O(n) |
| Binary search | O(log n) |
| Sorting | O(n log n) |
| HashMap/Set lookup | O(1) average |
| Two pointers | O(n) |
| Nested loops | O(n²) |
| Recursion (binary) | O(2^n) |

## Common Interview Question Patterns

### Two Pointer Pattern
- Palindrome checking
- Array reversal
- Two sum / pair sum
- Trapping rain water

### Sliding Window Pattern
- Longest substring problems
- Subarray problems
- Window-based iterations

### HashMap/Set Pattern
- Character frequency
- Duplicate detection
- Anagram checking

### Stack Pattern
- Balanced parentheses
- Expression evaluation
- Next greater element

### Tree Pattern
- Traversals (inorder, preorder, postorder)
- Level order (BFS)
- Path sum problems

### DP Pattern
- Fibonacci variants
- Coin change
- Longest subsequence

### Must Memorize
- Bubble Sort ✅
- Binary Search ✅
- Reverse String ✅
- Palindrome ✅
- Factorial (loop + recursion) ✅
- Frequency Map ✅
- Missing Number ✅

---

## Last-Minute Quick Tips

1. **Time is valuable** → Choose simple, correct solution over perfect
2. **Edge cases matter** → Empty array, single element, null values
3. **Variable names** → Use meaningful names (`leftMax` not `lm`)
4. **Explain as you code** → Talk through your logic
5. **Test with examples** → Manually trace through your code
6. **Optimize if needed** → But only if interviewer asks
7. **Complexity statement** → Always mention both time AND space

Good luck! Remember, practice makes perfect. Code regularly! 🚀
