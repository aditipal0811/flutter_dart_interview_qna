# Coding Round Questions - Dart Solutions With Easy Explanations

Use this file to prepare for coding interviews. For every question, remember this speaking format:

1. Tell the brute force idea if needed.
2. Tell the optimized idea.
3. Explain the loop/pointers/data structure.
4. Give time and space complexity.

---

## 1. Arrays

### 1.1 Find the second largest element in an array
**Idea:** Keep two variables: `largest` and `secondLargest`.

**Dart code:**
```dart
int? secondLargest(List<int> nums) {
  if (nums.length < 2) return null;

  int? largest;
  int? second;

  for (final num in nums) {
    if (largest == null || num > largest) {
      second = largest;
      largest = num;
    } else if (num != largest && (second == null || num > second)) {
      second = num;
    }
  }

  return second;
}
```

**Explain in interview:** I scan the array once. Whenever I find a new largest value, the old largest becomes second largest. If a value is smaller than largest but bigger than second largest, I update second largest.

**Complexity:** Time `O(n)`, Space `O(1)`.

### 1.2 Check if the array is sorted
**Idea:** Every element should be less than or equal to the next element.

```dart
bool isSorted(List<int> nums) {
  for (int i = 0; i < nums.length - 1; i++) {
    if (nums[i] > nums[i + 1]) return false;
  }
  return true;
}
```

**Explain:** I compare every pair of neighbors. If any previous element is greater than the next element, the array is not sorted.

**Complexity:** Time `O(n)`, Space `O(1)`.

### 1.3 Remove duplicates from a sorted array
**Idea:** Since the array is sorted, duplicates are side by side. Use one pointer to write unique values.

```dart
int removeDuplicates(List<int> nums) {
  if (nums.isEmpty) return 0;

  int write = 1;
  for (int read = 1; read < nums.length; read++) {
    if (nums[read] != nums[write - 1]) {
      nums[write] = nums[read];
      write++;
    }
  }
  return write;
}
```

**Explain:** `read` scans the array. `write` stores the next unique value position. At the end, the first `write` elements are unique.

**Complexity:** Time `O(n)`, Space `O(1)`.

### 1.4 Left rotate / Right rotate an array by D elements
**Idea:** Normalize `d` using modulo, then use slicing for easy interview code.

```dart
List<int> rotateLeft(List<int> nums, int d) {
  if (nums.isEmpty) return nums;
  d = d % nums.length;
  return [...nums.sublist(d), ...nums.sublist(0, d)];
}

List<int> rotateRight(List<int> nums, int d) {
  if (nums.isEmpty) return nums;
  d = d % nums.length;
  return [...nums.sublist(nums.length - d), ...nums.sublist(0, nums.length - d)];
}
```

**Explain:** For left rotation, elements after `d` come first and the first `d` elements go to the end. For right rotation, the last `d` elements come first.

**Complexity:** Time `O(n)`, Space `O(n)`. For in-place rotation, use reverse algorithm with `O(1)` space.

### 1.5 Move all zeros to the end
**Idea:** Move non-zero values to the front, then fill remaining positions with zero.

```dart
void moveZerosToEnd(List<int> nums) {
  int write = 0;

  for (final num in nums) {
    if (num != 0) {
      nums[write] = num;
      write++;
    }
  }

  while (write < nums.length) {
    nums[write] = 0;
    write++;
  }
}
```

**Explain:** I keep the order of non-zero elements. First I write all non-zero values, then I fill the remaining array with zeros.

**Complexity:** Time `O(n)`, Space `O(1)`.

### 1.6 Find the missing number in an array of 1 to N
**Idea:** Expected sum minus actual sum gives the missing number.

```dart
int missingNumber(List<int> nums, int n) {
  final expected = n * (n + 1) ~/ 2;
  final actual = nums.fold<int>(0, (sum, value) => sum + value);
  return expected - actual;
}
```

**Explain:** Sum of numbers from 1 to N is `n * (n + 1) / 2`. If one number is missing, the difference between expected sum and actual sum is the missing number.

**Complexity:** Time `O(n)`, Space `O(1)`.

### 1.7 Find the union and intersection of two arrays
**Idea:** Use sets.

```dart
Set<int> unionArray(List<int> a, List<int> b) {
  return {...a, ...b};
}

Set<int> intersectionArray(List<int> a, List<int> b) {
  final setA = a.toSet();
  return b.where((x) => setA.contains(x)).toSet();
}
```

**Explain:** A set stores unique values. For union, add both arrays to a set. For intersection, check which values from the second array exist in the first set.

**Complexity:** Time `O(n + m)`, Space `O(n + m)`.

### 1.8 Kadane's Algorithm (Maximum Subarray Sum)
**Idea:** At each index, decide whether to start a new subarray or extend the previous one.

```dart
int maxSubArray(List<int> nums) {
  int current = nums[0];
  int best = nums[0];

  for (int i = 1; i < nums.length; i++) {
    current = nums[i] > current + nums[i] ? nums[i] : current + nums[i];
    best = best > current ? best : current;
  }

  return best;
}
```

**Explain:** `current` stores the best subarray sum ending at current index. If previous sum becomes harmful, I start fresh from current number.

**Complexity:** Time `O(n)`, Space `O(1)`.

---

## 2. Strings

### 2.1 Reverse a string
```dart
String reverseString(String s) {
  return s.split('').reversed.join();
}
```

**Explain:** Convert the string to characters, reverse them, and join again.

**Complexity:** Time `O(n)`, Space `O(n)`.

### 2.2 Check if a string is a palindrome
**Idea:** Use two pointers from both ends.

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

**Explain:** A palindrome reads the same from both sides. I compare first with last, second with second-last, and so on.

**Complexity:** Time `O(n)`, Space `O(1)`.

### 2.3 Check if two strings are anagrams
**Idea:** Count characters.

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

**Explain:** Two anagrams have the same characters with the same frequency. I add counts from first string and subtract using second string.

**Complexity:** Time `O(n)`, Space `O(k)` where `k` is unique characters.

### 2.4 Find the first non-repeating character
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

**Explain:** First I count every character. Then I scan the string again and return the first character whose count is one.

**Complexity:** Time `O(n)`, Space `O(k)`.

### 2.5 Remove duplicate characters from a string
```dart
String removeDuplicateChars(String s) {
  final seen = <String>{};
  final result = StringBuffer();

  for (final ch in s.split('')) {
    if (seen.add(ch)) result.write(ch);
  }

  return result.toString();
}
```

**Explain:** A set tells whether I have already seen a character. I add only the first occurrence to the result.

**Complexity:** Time `O(n)`, Space `O(k)`.

### 2.6 Count vowels and consonants in a string
```dart
Map<String, int> countVowelsAndConsonants(String s) {
  final vowels = {'a', 'e', 'i', 'o', 'u'};
  int vowelCount = 0;
  int consonantCount = 0;

  for (final ch in s.toLowerCase().split('')) {
    if (RegExp(r'[a-z]').hasMatch(ch)) {
      if (vowels.contains(ch)) {
        vowelCount++;
      } else {
        consonantCount++;
      }
    }
  }

  return {'vowels': vowelCount, 'consonants': consonantCount};
}
```

**Explain:** I ignore non-alphabet characters. For letters, I check whether the character is a vowel or consonant.

**Complexity:** Time `O(n)`, Space `O(1)`.

### 2.7 Longest common prefix among strings
```dart
String longestCommonPrefix(List<String> words) {
  if (words.isEmpty) return '';

  String prefix = words[0];
  for (int i = 1; i < words.length; i++) {
    while (!words[i].startsWith(prefix)) {
      prefix = prefix.substring(0, prefix.length - 1);
      if (prefix.isEmpty) return '';
    }
  }

  return prefix;
}
```

**Explain:** I start with the first word as prefix. If another word does not start with it, I keep reducing the prefix until it matches.

**Complexity:** Time `O(n * m)`, Space `O(1)`.

### 2.8 Implement atoi() / myAtoi()
**Idea:** Handle spaces, sign, and digits.

```dart
int myAtoi(String s) {
  int i = 0;
  int sign = 1;
  int result = 0;

  while (i < s.length && s[i] == ' ') i++;

  if (i < s.length && (s[i] == '+' || s[i] == '-')) {
    sign = s[i] == '-' ? -1 : 1;
    i++;
  }

  while (i < s.length) {
    final code = s.codeUnitAt(i);
    if (code < 48 || code > 57) break;
    result = result * 10 + (code - 48);
    i++;
  }

  return result * sign;
}
```

**Explain:** I skip spaces, read optional sign, then build the number digit by digit until I find a non-digit character.

**Complexity:** Time `O(n)`, Space `O(1)`.

---

## 3. Pattern Questions

Pattern questions mainly test nested loops. Explain rows, columns, and print condition.

Helper used in some examples:

```dart
String repeatText(String value, int count) {
  return List.filled(count, value).join();
}
```

### 3.1 Print solid patterns
```dart
void solidSquare(int n) {
  for (int i = 0; i < n; i++) {
    print(repeatText('* ', n));
  }
}
```

**Explain:** Outer loop controls rows. Inner printing controls columns.

### 3.2 Print hollow patterns
```dart
void hollowSquare(int n) {
  for (int i = 0; i < n; i++) {
    String row = '';
    for (int j = 0; j < n; j++) {
      if (i == 0 || i == n - 1 || j == 0 || j == n - 1) {
        row += '* ';
      } else {
        row += '  ';
      }
    }
    print(row);
  }
}
```

**Explain:** Print star on boundary rows and boundary columns. Print spaces inside.

### 3.3 Number patterns
```dart
void numberTriangle(int n) {
  for (int i = 1; i <= n; i++) {
    String row = '';
    for (int j = 1; j <= i; j++) {
      row += '$j ';
    }
    print(row);
  }
}
```

### 3.4 Floyd's Triangle
```dart
void floydTriangle(int n) {
  int value = 1;
  for (int i = 1; i <= n; i++) {
    String row = '';
    for (int j = 1; j <= i; j++) {
      row += '${value++} ';
    }
    print(row);
  }
}
```

### 3.5 Inverted patterns
```dart
void invertedTriangle(int n) {
  for (int i = n; i >= 1; i--) {
    print(repeatText('* ', i));
  }
}
```

### 3.6 Butterfly pattern
```dart
void butterfly(int n) {
  for (int i = 1; i <= n; i++) {
    print(
      repeatText('*', i) +
          repeatText(' ', 2 * (n - i)) +
          repeatText('*', i),
    );
  }
  for (int i = n; i >= 1; i--) {
    print(
      repeatText('*', i) +
          repeatText(' ', 2 * (n - i)) +
          repeatText('*', i),
    );
  }
}
```

### 3.7 Diamond pattern
```dart
void diamond(int n) {
  for (int i = 1; i <= n; i++) {
    print(repeatText(' ', n - i) + repeatText('* ', i));
  }
  for (int i = n - 1; i >= 1; i--) {
    print(repeatText(' ', n - i) + repeatText('* ', i));
  }
}
```

**Pattern complexity:** Usually Time `O(n^2)`, Space `O(1)` excluding printed output.

---

## 4. Number Problems

### 4.1 Check if a number is prime
```dart
bool isPrime(int n) {
  if (n <= 1) return false;
  for (int i = 2; i * i <= n; i++) {
    if (n % i == 0) return false;
  }
  return true;
}
```

**Explain:** I only check up to square root because if a number has a larger factor, it must also have a smaller paired factor.

**Complexity:** Time `O(sqrt(n))`, Space `O(1)`.

### 4.2 Find GCD / HCF and LCM
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

**Explain:** GCD uses Euclidean algorithm. LCM formula is `a * b / gcd`.

**Complexity:** Time `O(log(min(a, b)))`, Space `O(1)`.

### 4.3 Check if a number is palindrome
```dart
bool isNumberPalindrome(int n) {
  final original = n;
  int reversed = 0;

  while (n > 0) {
    reversed = reversed * 10 + n % 10;
    n ~/= 10;
  }

  return original == reversed;
}
```

**Explain:** Reverse the number and compare it with the original number.

### 4.4 Armstrong number
```dart
bool isArmstrong(int n) {
  final original = n;
  final digits = n.toString().length;
  int sum = 0;

  while (n > 0) {
    final digit = n % 10;
    sum += powInt(digit, digits);
    n ~/= 10;
  }

  return sum == original;
}

int powInt(int base, int exp) {
  int result = 1;
  for (int i = 0; i < exp; i++) {
    result *= base;
  }
  return result;
}
```

**Explain:** An Armstrong number equals the sum of each digit raised to the power of total digits.

### 4.5 Fibonacci series
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

**Explain:** Every Fibonacci number is the sum of the previous two numbers.

### 4.6 Count digits in a number
```dart
int countDigits(int n) {
  if (n == 0) return 1;
  n = n.abs();
  int count = 0;
  while (n > 0) {
    count++;
    n ~/= 10;
  }
  return count;
}
```

### 4.7 Reverse a number
```dart
int reverseNumber(int n) {
  int reversed = 0;
  while (n > 0) {
    reversed = reversed * 10 + n % 10;
    n ~/= 10;
  }
  return reversed;
}
```

---

## 5. Recursion

**How to explain recursion:** A recursive function has a base case to stop and a recursive call to solve a smaller problem.

### 5.1 Factorial of a number
```dart
int factorial(int n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}
```

**Explain:** `n! = n * (n - 1)!`. Base case is `0!` or `1! = 1`.

### 5.2 Fibonacci series (Recursive)
```dart
int fib(int n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
}
```

**Explain:** This is simple but not optimal because it recalculates values. Optimized version uses DP/memoization.

### 5.3 Check if a string is palindrome (Recursive)
```dart
bool recursivePalindrome(String s, int left, int right) {
  if (left >= right) return true;
  if (s[left] != s[right]) return false;
  return recursivePalindrome(s, left + 1, right - 1);
}
```

### 5.4 Print all subsequences of a string
```dart
void printSubsequences(String s, int index, String current) {
  if (index == s.length) {
    print(current);
    return;
  }

  printSubsequences(s, index + 1, current + s[index]);
  printSubsequences(s, index + 1, current);
}
```

**Explain:** For every character, we have two choices: include it or exclude it.

**Complexity:** Time `O(2^n)`.

### 5.5 Tower of Hanoi
```dart
void towerOfHanoi(int n, String from, String helper, String to) {
  if (n == 0) return;

  towerOfHanoi(n - 1, from, to, helper);
  print('Move disk $n from $from to $to');
  towerOfHanoi(n - 1, helper, from, to);
}
```

**Explain:** Move `n - 1` disks to helper, move largest disk to target, then move `n - 1` disks from helper to target.

### 5.6 Sum of digits of a number (Recursive)
```dart
int sumDigits(int n) {
  if (n == 0) return 0;
  return n % 10 + sumDigits(n ~/ 10);
}
```

---

## 6. Stack & Queue

### 6.1 Implement Stack using array
```dart
class Stack<T> {
  final List<T> _items = [];

  void push(T value) => _items.add(value);

  T? pop() => _items.isEmpty ? null : _items.removeLast();

  T? peek() => _items.isEmpty ? null : _items.last;

  bool get isEmpty => _items.isEmpty;
}
```

**Explain:** Stack follows LIFO: last in, first out.

### 6.2 Implement Queue using array
```dart
class Queue<T> {
  final List<T> _items = [];

  void enqueue(T value) => _items.add(value);

  T? dequeue() => _items.isEmpty ? null : _items.removeAt(0);

  bool get isEmpty => _items.isEmpty;
}
```

**Explain:** Queue follows FIFO: first in, first out. In production, use `ListQueue` from `dart:collection` for efficient dequeue.

### 6.3 Check for balanced parentheses
```dart
bool isBalanced(String s) {
  final stack = <String>[];
  final pairs = {')': '(', ']': '[', '}': '{'};

  for (final ch in s.split('')) {
    if (pairs.containsValue(ch)) {
      stack.add(ch);
    } else if (pairs.containsKey(ch)) {
      if (stack.isEmpty || stack.removeLast() != pairs[ch]) {
        return false;
      }
    }
  }

  return stack.isEmpty;
}
```

**Explain:** Push opening brackets. For every closing bracket, top of stack must be its matching opening bracket.

**Complexity:** Time `O(n)`, Space `O(n)`.

### 6.4 Infix to Postfix conversion (Shunting-Yard)
```dart
int precedence(String op) {
  if (op == '+' || op == '-') return 1;
  if (op == '*' || op == '/') return 2;
  return 0;
}

String infixToPostfix(String expression) {
  final stack = <String>[];
  final output = StringBuffer();

  for (final ch in expression.replaceAll(' ', '').split('')) {
    if (RegExp(r'[a-zA-Z0-9]').hasMatch(ch)) {
      output.write(ch);
    } else if (ch == '(') {
      stack.add(ch);
    } else if (ch == ')') {
      while (stack.isNotEmpty && stack.last != '(') {
        output.write(stack.removeLast());
      }
      stack.removeLast();
    } else {
      while (stack.isNotEmpty && precedence(stack.last) >= precedence(ch)) {
        output.write(stack.removeLast());
      }
      stack.add(ch);
    }
  }

  while (stack.isNotEmpty) {
    output.write(stack.removeLast());
  }

  return output.toString();
}
```

**Explain:** Operators wait in stack until their priority allows them to come out. Operands go directly to output.

### 6.5 Evaluate Postfix expression
```dart
int evaluatePostfix(String expression) {
  final stack = <int>[];

  for (final ch in expression.split('')) {
    if (RegExp(r'\d').hasMatch(ch)) {
      stack.add(int.parse(ch));
    } else {
      final b = stack.removeLast();
      final a = stack.removeLast();
      if (ch == '+') stack.add(a + b);
      if (ch == '-') stack.add(a - b);
      if (ch == '*') stack.add(a * b);
      if (ch == '/') stack.add(a ~/ b);
    }
  }

  return stack.last;
}
```

**Explain:** In postfix, operator comes after operands. So when I see an operator, I pop two values, calculate, and push result.

---

## 7. Searching & Sorting

### 7.1 Binary Search
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

**Explain:** Binary search works only on sorted arrays. It cuts the search space in half each time.

**Complexity:** Time `O(log n)`, Space `O(1)`.

### 7.2 Linear Search
```dart
int linearSearch(List<int> nums, int target) {
  for (int i = 0; i < nums.length; i++) {
    if (nums[i] == target) return i;
  }
  return -1;
}
```

**Complexity:** Time `O(n)`, Space `O(1)`.

### 7.3 Bubble Sort
```dart
void bubbleSort(List<int> nums) {
  for (int i = 0; i < nums.length; i++) {
    bool swapped = false;
    for (int j = 0; j < nums.length - i - 1; j++) {
      if (nums[j] > nums[j + 1]) {
        final temp = nums[j];
        nums[j] = nums[j + 1];
        nums[j + 1] = temp;
        swapped = true;
      }
    }
    if (!swapped) break;
  }
}
```

**Explain:** Bigger elements bubble to the end after each pass.

**Complexity:** Time `O(n^2)`, Space `O(1)`.

### 7.4 Selection Sort
```dart
void selectionSort(List<int> nums) {
  for (int i = 0; i < nums.length; i++) {
    int minIndex = i;
    for (int j = i + 1; j < nums.length; j++) {
      if (nums[j] < nums[minIndex]) minIndex = j;
    }
    final temp = nums[i];
    nums[i] = nums[minIndex];
    nums[minIndex] = temp;
  }
}
```

**Explain:** Find the minimum element from unsorted part and place it at the current position.

**Complexity:** Time `O(n^2)`, Space `O(1)`.

### 7.5 Insertion Sort
```dart
void insertionSort(List<int> nums) {
  for (int i = 1; i < nums.length; i++) {
    final key = nums[i];
    int j = i - 1;

    while (j >= 0 && nums[j] > key) {
      nums[j + 1] = nums[j];
      j--;
    }
    nums[j + 1] = key;
  }
}
```

**Explain:** Take one element and insert it in the correct position in the sorted left part.

**Complexity:** Time `O(n^2)`, Space `O(1)`.

### 7.6 Merge Sort
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
  int i = 0;
  int j = 0;

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

**Explain:** Divide the array into halves, sort each half, then merge two sorted arrays.

**Complexity:** Time `O(n log n)`, Space `O(n)`.

### 7.7 Quick Sort
```dart
List<int> quickSort(List<int> nums) {
  if (nums.length <= 1) return nums;

  final pivot = nums[nums.length ~/ 2];
  final less = nums.where((x) => x < pivot).toList();
  final equal = nums.where((x) => x == pivot).toList();
  final greater = nums.where((x) => x > pivot).toList();

  return [...quickSort(less), ...equal, ...quickSort(greater)];
}
```

**Explain:** Choose a pivot, put smaller elements on left and greater elements on right, then sort both sides recursively.

**Complexity:** Average Time `O(n log n)`, Worst Time `O(n^2)`, Space depends on implementation.

---

## 8. Common Interview Problems

### 8.1 Two Sum Problem
**Idea:** Use a map to store required complement.

```dart
List<int> twoSum(List<int> nums, int target) {
  final seen = <int, int>{};

  for (int i = 0; i < nums.length; i++) {
    final need = target - nums[i];
    if (seen.containsKey(need)) {
      return [seen[need]!, i];
    }
    seen[nums[i]] = i;
  }

  return [];
}
```

**Explain:** For every number, I check whether the number needed to reach target already exists in the map.

**Complexity:** Time `O(n)`, Space `O(n)`.

### 8.2 Check if array contains a pair with given sum
```dart
bool hasPairWithSum(List<int> nums, int target) {
  final seen = <int>{};

  for (final num in nums) {
    if (seen.contains(target - num)) return true;
    seen.add(num);
  }

  return false;
}
```

**Explain:** Same as Two Sum, but I only return true or false.

### 8.3 Find majority element (Boyer-Moore Voting)
**Idea:** Majority element appears more than `n / 2` times.

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

**Explain:** Opposite elements cancel each other. If a majority exists, it will remain as candidate at the end. Then I verify it.

**Complexity:** Time `O(n)`, Space `O(1)`.

### 8.4 Leaders in an array
**Definition:** An element is a leader if no element on its right is greater than it.

```dart
List<int> leaders(List<int> nums) {
  final result = <int>[];
  int maxRight = nums.last;

  for (int i = nums.length - 1; i >= 0; i--) {
    if (nums[i] >= maxRight) {
      result.add(nums[i]);
      maxRight = nums[i];
    }
  }

  return result.reversed.toList();
}
```

**Explain:** I scan from right because I need to know the maximum value on the right side.

**Complexity:** Time `O(n)`, Space `O(n)` for output.

### 8.5 Trapping Rain Water
**Idea:** Water at any index depends on left max and right max.

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

**Explain:** I use two pointers. The smaller side decides the water because water is limited by the shorter boundary.

**Complexity:** Time `O(n)`, Space `O(1)`.

### 8.6 Maximum / Minimum in sliding window
**Simple version:** For each window, calculate max/min.

```dart
List<int> maxSlidingWindowSimple(List<int> nums, int k) {
  final result = <int>[];

  for (int i = 0; i <= nums.length - k; i++) {
    int maxValue = nums[i];
    for (int j = i; j < i + k; j++) {
      if (nums[j] > maxValue) maxValue = nums[j];
    }
    result.add(maxValue);
  }

  return result;
}
```

**Optimized idea:** Use a deque to keep useful indices in decreasing order. The front always contains the maximum for the current window.

```dart
List<int> maxSlidingWindow(List<int> nums, int k) {
  final result = <int>[];
  final deque = <int>[];

  for (int i = 0; i < nums.length; i++) {
    while (deque.isNotEmpty && deque.first <= i - k) {
      deque.removeAt(0);
    }

    while (deque.isNotEmpty && nums[deque.last] <= nums[i]) {
      deque.removeLast();
    }

    deque.add(i);

    if (i >= k - 1) {
      result.add(nums[deque.first]);
    }
  }

  return result;
}
```

**Explain:** I remove indices that are outside the window. I also remove smaller values from the back because they can never become maximum while the current bigger value is present.

**Complexity:** Simple Time `O(n * k)`. Optimized Time `O(n)`, Space `O(k)`.

---

## Last-Minute Complexity Revision

- Single loop: `O(n)`
- Nested loop over same array: usually `O(n^2)`
- Binary search: `O(log n)`
- Sorting: usually `O(n log n)`
- HashMap/Set lookup: average `O(1)`
- Recursion with two choices each step: usually `O(2^n)`
- Stack/Queue push and pop: `O(1)`

## How To Speak During Coding Round

Use this simple sentence pattern:

**"I will solve this using [technique]. I will maintain [variables/data structure]. I will scan [direction]. This gives time complexity [time] and space complexity [space]."**

Example:

**"I will solve Two Sum using a HashMap. I will store visited numbers with their indexes. For every number, I will check if target minus current number already exists. This gives O(n) time and O(n) space."**
