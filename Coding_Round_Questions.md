# Coding Round Questions & Answers

This document provides optimized Dart solutions for common data structures and algorithms questions, complete with complexity analysis and professional explanations.

---

## 1. Arrays

---

### 1.1 Find the second largest element in an array
**Dart Code:**
```dart
void main() {
  List<int> numbers = [12, 35, 1, 10, 34, 1];
  int? secondLargest = findSecondLargest(numbers);
  print("The second largest element is: $secondLargest"); // Output: 34
}

int? findSecondLargest(List<int> list) {
  if (list.length < 2) return null;

  int largest = -2147483648;
  int secondLargest = -2147483648;

  for (int number in list) {
    if (number > largest) {
      secondLargest = largest;
      largest = number;
    } else if (number > secondLargest && number != largest) {
      secondLargest = number;
    }
  }

  return secondLargest == -2147483648 ? null : secondLargest;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Single-pass linear scan.
- **Space Complexity:** $O(1)$ - Constant auxiliary memory.

**Algorithmic Logic:**
We track the largest and second largest elements using two variables. During iteration, if the current element exceeds `largest`, the previous `largest` becomes the `secondLargest`, and `largest` is updated. If the element is smaller than `largest` but greater than `secondLargest`, we update `secondLargest`.

---

### 1.2 Check if the array is sorted
**Dart Code:**
```dart
void main() {
  List<int> numbers = [2, 5, 8, 12, 15];
  print("Is sorted? ${isSorted(numbers)}"); // Output: true
}

bool isSorted(List<int> list) {
  for (int i = 0; i < list.length - 1; i++) {
    if (list[i] > list[i + 1]) {
      return false;
    }
  }
  return true;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Single scan.
- **Space Complexity:** $O(1)$ - Constant memory.

**Algorithmic Logic:**
A list is sorted in non-decreasing order if every element is less than or equal to the element directly succeeding it. We perform a linear scan and check if $A[i] > A[i+1]$. If this condition is met at any point, the array is unsorted.

---

### 1.3 Remove duplicates from a sorted array
**Dart Code:**
```dart
void main() {
  List<int> numbers = [1, 1, 2, 2, 3, 4, 4];
  int newLength = removeDuplicates(numbers);
  print("Unique array: ${numbers.sublist(0, newLength)}"); // Output: [1, 2, 3, 4]
}

int removeDuplicates(List<int> list) {
  if (list.isEmpty) return 0;
  
  int uniqueIndex = 0;
  
  for (int i = 1; i < list.length; i++) {
    if (list[i] != list[uniqueIndex]) {
      uniqueIndex++;
      list[uniqueIndex] = list[i];
    }
  }
  return uniqueIndex + 1;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Single pass using two pointers.
- **Space Complexity:** $O(1)$ - In-place modification.

**Algorithmic Logic:**
Since the array is sorted, duplicates are adjacent. We maintain a pointer `uniqueIndex` representing the write barrier for unique elements. We iterate through the array starting from the second element, and whenever we find an element different from the one at `uniqueIndex`, we increment `uniqueIndex` and copy the element over.

---

### 1.4 Left rotate / Right rotate an array by D elements
**Dart Code:**
```dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  
  print("Left rotated by 2: ${rotateLeft(List.from(numbers), 2)}");   // Output: [3, 4, 5, 1, 2]
  print("Right rotated by 2: ${rotateRight(List.from(numbers), 2)}"); // Output: [4, 5, 1, 2, 3]
}

List<int> rotateLeft(List<int> list, int d) {
  int n = list.length;
  if (n == 0) return list;
  d = d % n;
  
  reverse(list, 0, d - 1);
  reverse(list, d, n - 1);
  reverse(list, 0, n - 1);
  return list;
}

List<int> rotateRight(List<int> list, int d) {
  int n = list.length;
  if (n == 0) return list;
  d = d % n;
  
  reverse(list, 0, n - 1);
  reverse(list, 0, d - 1);
  reverse(list, d, n - 1);
  return list;
}

void reverse(List<int> list, int start, int end) {
  while (start < end) {
    int temp = list[start];
    list[start] = list[end];
    list[end] = temp;
    start++;
    end--;
  }
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Three reversal operations, each taking linear time.
- **Space Complexity:** $O(1)$ - In-place reversal.

**Algorithmic Logic:**
Instead of shifting elements individually or using auxiliary memory, we use the Reversal Algorithm. For left rotation by `D` steps:
1. Reverse the prefix subarray $0$ to $D-1$.
2. Reverse the suffix subarray $D$ to $N-1$.
3. Reverse the entire array.

---

### 1.5 Move all zeros to the end
**Dart Code:**
```dart
void main() {
  List<int> numbers = [0, 1, 0, 3, 12];
  moveZerosToEnd(numbers);
  print("Result: $numbers"); // Output: [1, 3, 12, 0, 0]
}

void moveZerosToEnd(List<int> list) {
  int nonZeroPos = 0;
  
  for (int i = 0; i < list.length; i++) {
    if (list[i] != 0) {
      int temp = list[i];
      list[i] = list[nonZeroPos];
      list[nonZeroPos] = temp;
      nonZeroPos++;
    }
  }
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Single-pass traversal.
- **Space Complexity:** $O(1)$ - In-place swaps.

**Algorithmic Logic:**
We track the position of the next non-zero element using `nonZeroPos`. Whenever we encounter a non-zero element during our iteration, we swap it with the element at `nonZeroPos` and increment `nonZeroPos`. All zeros are thus naturally partitioned to the right side of the list.

---

### 1.6 Find the missing number in an array of 1 to N
**Dart Code:**
```dart
void main() {
  List<int> numbers = [1, 2, 4, 5, 6]; // N is 6, 3 is missing
  int missing = findMissingNumber(numbers, 6);
  print("The missing number is: $missing"); // Output: 3
}

int findMissingNumber(List<int> list, int n) {
  int expectedSum = (n * (n + 1)) ~/ 2; 
  int actualSum = 0;
  for (int num in list) {
    actualSum += num;
  }
  return expectedSum - actualSum;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Linear sum calculation.
- **Space Complexity:** $O(1)$ - Constant space.

**Algorithmic Logic:**
We use the mathematical formula for the sum of the first $N$ natural numbers: $S_n = \frac{N(N+1)}{2}$. By subtracting the actual sum of elements present in the list from the expected mathematical sum, we isolate the missing number.

---

### 1.7 Find the union and intersection of two arrays
**Dart Code:**
```dart
void main() {
  List<int> list1 = [1, 2, 2, 3, 4];
  List<int> list2 = [2, 2, 4, 5];
  
  print("Union: ${findUnion(list1, list2)}");               // Output: [1, 2, 3, 4, 5]
  print("Intersection: ${findIntersection(list1, list2)}");   // Output: [2, 4]
}

List<int> findUnion(List<int> a, List<int> b) {
  Set<int> unionSet = {};
  unionSet.addAll(a);
  unionSet.addAll(b);
  return unionSet.toList();
}

List<int> findIntersection(List<int> a, List<int> b) {
  Set<int> setA = a.toSet();
  Set<int> intersectionSet = {};
  
  for (int item in b) {
    if (setA.contains(item)) {
      intersectionSet.add(item);
    }
  }
  return intersectionSet.toList();
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N + M)$ - Linear insertion and hash lookup.
- **Space Complexity:** $O(N + M)$ - Storing unique items in Set.

**Algorithmic Logic:**
- **Union:** We leverage the unique property of `Set` structures to eliminate duplicates and merge elements of both arrays.
- **Intersection:** We convert the first list into a hash set to achieve $O(1)$ lookups. We then iterate through the second list, checking if the elements exist in the hash set to determine common elements.

---

### 1.8 Kadane's Algorithm (Maximum Subarray Sum)
**Dart Code:**
```dart
void main() {
  List<int> numbers = [-2, 1, -3, 4, -1, 2, 1, -5, 4];
  print("Max subarray sum: ${maxSubarraySum(numbers)}"); // Output: 6 ([4, -1, 2, 1])
}

int maxSubarraySum(List<int> list) {
  if (list.isEmpty) return 0;
  
  int maxSoFar = list[0];
  int currentSum = list[0];
  
  for (int i = 1; i < list.length; i++) {
    currentSum = (list[i] > currentSum + list[i]) ? list[i] : currentSum + list[i];
    if (currentSum > maxSoFar) {
      maxSoFar = currentSum;
    }
  }
  return maxSoFar;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Single pass.
- **Space Complexity:** $O(1)$ - Constant auxiliary variables.

**Algorithmic Logic:**
We keep track of the maximum sum of a contiguous subarray ending at the current index. For each element, we decide whether to add it to the existing subarray sum or start a new subarray sum from the current element. We update the global maximum whenever our local running sum exceeds it.

---

## 2. Strings

---

### 2.1 Reverse a string
**Dart Code:**
```dart
void main() {
  String text = "hello";
  print("Reversed: ${reverseString(text)}"); // Output: "olleh"
}

String reverseString(String input) {
  List<String> characters = input.split('');
  int start = 0;
  int end = characters.length - 1;
  
  while (start < end) {
    String temp = characters[start];
    characters[start] = characters[end];
    characters[end] = temp;
    start++;
    end--;
  }
  return characters.join('');
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Linear time swap operations.
- **Space Complexity:** $O(N)$ - Storing character array.

**Algorithmic Logic:**
We convert the string into a list of characters and apply the two-pointer technique. We swap characters at the `start` and `end` indices, then increment `start` and decrement `end` until they meet.

---

### 2.2 Check if a string is a palindrome
**Dart Code:**
```dart
void main() {
  print("Is 'racecar' a palindrome? ${isPalindrome('racecar')}"); // Output: true
}

bool isPalindrome(String input) {
  int start = 0;
  int end = input.length - 1;
  
  while (start < end) {
    if (input[start] != input[end]) {
      return false;
    }
    start++;
    end--;
  }
  return true;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Linear check.
- **Space Complexity:** $O(1)$ - Constant pointers.

**Algorithmic Logic:**
Using two pointers (one at the beginning and one at the end), we move inwards step-by-step comparing characters. If any mismatch occurs, the string is not a palindrome.

---

### 2.3 Check if two strings are anagrams
**Dart Code:**
```dart
void main() {
  print("Are 'silent' and 'listen' anagrams? ${isAnagram('silent', 'listen')}"); // Output: true
}

bool isAnagram(String a, String b) {
  if (a.length != b.length) return false;
  
  List<String> listA = a.split('')..sort();
  List<String> listB = b.split('')..sort();
  
  for (int i = 0; i < listA.length; i++) {
    if (listA[i] != listB[i]) return false;
  }
  return true;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N \log N)$ - Dominated by sorting characters.
- **Space Complexity:** $O(N)$ - Storing sorted lists.

**Algorithmic Logic:**
An anagram must contain the exact same characters in the exact same frequencies. By sorting the character representation of both strings alphabetically, we align matching characters. If the sorted lists are identical, the strings are anagrams.

---

### 2.4 Find the first non-repeating character
**Dart Code:**
```dart
void main() {
  String word = "swiss";
  print("First non-repeating char: ${firstNonRepeatingChar(word)}"); // Output: 'w'
}

String? firstNonRepeatingChar(String text) {
  Map<String, int> letterCounts = {};
  
  for (int i = 0; i < text.length; i++) {
    String char = text[i];
    letterCounts[char] = (letterCounts[char] ?? 0) + 1;
  }
  
  for (int i = 0; i < text.length; i++) {
    String char = text[i];
    if (letterCounts[char] == 1) {
      return char;
    }
  }
  return null;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Two linear passes (one to populate counts, one to find first single occurrence).
- **Space Complexity:** $O(k)$ - Where $k$ is the alphabet size, bounded by $O(1)$ for fixed alphabets.

**Algorithmic Logic:**
We calculate the frequency of each character in a single pass using a hash map. In the second pass, we iterate through the original string and look up frequencies. The first character with a frequency of `1` is returned.

---

### 2.5 Remove duplicate characters from a string
**Dart Code:**
```dart
void main() {
  print("Cleaned: ${removeDuplicateChars('programming')}"); // Output: "progamin"
}

String removeDuplicateChars(String input) {
  Set<String> seen = {};
  List<String> result = [];
  
  for (int i = 0; i < input.length; i++) {
    String char = input[i];
    if (!seen.contains(char)) {
      seen.add(char);
      result.add(char);
    }
  }
  return result.join('');
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Linear insertion and lookup.
- **Space Complexity:** $O(k)$ - Storing unique characters in Set.

**Algorithmic Logic:**
We traverse the string while maintaining a `seen` set. For each character, we verify if it is in the set. If it is absent, we record it in both the set and the output list, preventing duplicate preservation.

---

### 2.6 Count vowels and consonants in a string
**Dart Code:**
```dart
void main() {
  countVowelsAndConsonants("Hello World!"); 
  // Output: Vowels: 3, Consonants: 7
}

void countVowelsAndConsonants(String text) {
  int vowels = 0;
  int consonants = 0;
  String vowelsList = "aeiouAEIOU";
  
  for (int i = 0; i < text.length; i++) {
    String char = text[i];
    if (RegExp(r'[a-zA-Z]').hasMatch(char)) {
      if (vowelsList.contains(char)) {
        vowels++;
      } else {
        consonants++;
      }
    }
  }
  print("Vowels: $vowels, Consonants: $consonants");
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Linear string iteration.
- **Space Complexity:** $O(1)$ - Constant counter variables.

**Algorithmic Logic:**
Iterating over the string, we perform bounds-checking using regex to match alphabetical characters. If a character is alphabetic, we test if it exists within our vowel list to decide whether to increment `vowels` or `consonants`.

---

### 2.7 Longest common prefix among strings
**Dart Code:**
```dart
void main() {
  List<String> words = ["flower", "flow", "flight"];
  print("Common Prefix: ${longestCommonPrefix(words)}"); // Output: "fl"
}

String longestCommonPrefix(List<String> words) {
  if (words.isEmpty) return "";
  
  String prefix = words[0];
  
  for (int i = 1; i < words.length; i++) {
    while (!words[i].startsWith(prefix)) {
      prefix = prefix.substring(0, prefix.length - 1);
      if (prefix.isEmpty) return "";
    }
  }
  return prefix;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(S)$ - Where $S$ is the sum of characters across all strings.
- **Space Complexity:** $O(1)$ - Modifies search prefix in-place.

**Algorithmic Logic:**
We initialize our search space by setting the target prefix as the first string in the array. We then iteratively compare the prefix against each subsequent string. If a string does not start with the prefix, we reduce the prefix length from the right until it does, or until the prefix is empty.

---

### 2.8 Implement atoi() / myAtoi()
**Dart Code:**
```dart
void main() {
  print(myAtoi("   -42")); // Output: -42
  print(myAtoi("4193 with words")); // Output: 4193
}

int myAtoi(String s) {
  s = s.trim();
  if (s.isEmpty) return 0;
  
  int sign = 1;
  int index = 0;
  
  if (s[0] == '-') {
    sign = -1;
    index++;
  } else if (s[0] == '+') {
    index++;
  }
  
  double result = 0;
  
  while (index < s.length) {
    int codeUnit = s.codeUnitAt(index);
    if (codeUnit >= 48 && codeUnit <= 57) {
      int digit = codeUnit - 48;
      result = (result * 10) + digit;
    } else {
      break;
    }
    index++;
  }
  
  double finalVal = result * sign;
  
  int minInt = -2147483648;
  int maxInt = 2147483647;
  if (finalVal < minInt) return minInt;
  if (finalVal > maxInt) return maxInt;
  
  return finalVal.toInt();
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Linear traversal over numerical tokens.
- **Space Complexity:** $O(1)$ - Constant helper storage.

**Algorithmic Logic:**
We discard leading whitespace and determine if the next token is a sign operator (`+` or `-`). Then, we convert the numeric characters to their digital equivalents ($character\_code - 48$). We build the integer iteratively ($result = result \times 10 + digit$) while checking constraints to prevent 32-bit integer overflow.

---

## 3. Patterns

---

### 3.1 Print solid patterns
**Dart Code:**
```dart
void main() {
  printSolidSquare(4);
}

void printSolidSquare(int size) {
  for (int row = 0; row < size; row++) {
    String line = "";
    for (int col = 0; col < size; col++) {
      line += "* ";
    }
    print(line);
  }
}
```
**Output:**
```
* * * * 
* * * * 
* * * * 
* * * * 
```
**Complexity Analysis:**
- **Time Complexity:** $O(N^2)$ - Nested iteration.
- **Space Complexity:** $O(N)$ - Row output assembly.

---

### 3.2 Print hollow patterns
**Dart Code:**
```dart
void main() {
  printHollowSquare(5);
}

void printHollowSquare(int size) {
  for (int row = 0; row < size; row++) {
    String line = "";
    for (int col = 0; col < size; col++) {
      if (row == 0 || row == size - 1 || col == 0 || col == size - 1) {
        line += "* ";
      } else {
        line += "  ";
      }
    }
    print(line);
  }
}
```
**Output:**
```
* * * * * 
*       * 
*       * 
*       * 
* * * * * 
```
**Complexity Analysis:**
- **Time Complexity:** $O(N^2)$ - Double loop.
- **Space Complexity:** $O(N)$ - Row output assembly.

---

### 3.3 Number patterns
**Dart Code:**
```dart
void main() {
  printNumberPattern(4);
}

void printNumberPattern(int rows) {
  for (int row = 1; row <= rows; row++) {
    String line = "";
    for (int col = 1; col <= row; col++) {
      line += "$col ";
    }
    print(line);
  }
}
```
**Output:**
```
1 
1 2 
1 2 3 
1 2 3 4 
```
**Complexity Analysis:**
- **Time Complexity:** $O(N^2)$ - Nested loops.
- **Space Complexity:** $O(N)$ - Buffer string.

---

### 3.4 Floyd's Triangle
**Dart Code:**
```dart
void main() {
  printFloydsTriangle(4);
}

void printFloydsTriangle(int rows) {
  int count = 1;
  for (int row = 1; row <= rows; row++) {
    String line = "";
    for (int col = 1; col <= row; col++) {
      line += "$count ";
      count++;
    }
    print(line);
  }
}
```
**Output:**
```
1 
2 3 
4 5 6 
7 8 9 10 
```
**Complexity Analysis:**
- **Time Complexity:** $O(N^2)$ - Nested iteration.
- **Space Complexity:** $O(N)$ - Buffer string.

---

### 3.5 Inverted patterns
**Dart Code:**
```dart
void main() {
  printInvertedTriangle(4);
}

void printInvertedTriangle(int rows) {
  for (int row = rows; row >= 1; row--) {
    String line = "";
    for (int col = 1; col <= row; col++) {
      line += "* ";
    }
    print(line);
  }
}
```
**Output:**
```
* * * * 
* * * 
* * 
* 
```
**Complexity Analysis:**
- **Time Complexity:** $O(N^2)$ - Nested iteration.
- **Space Complexity:** $O(N)$ - Line layout.

---

### 3.6 Butterfly pattern
**Dart Code:**
```dart
void main() {
  printButterfly(4);
}

void printButterfly(int n) {
  for (int i = 1; i <= n; i++) {
    String leftStars = "* " * i;
    String spaces = "  " * (2 * (n - i));
    String rightStars = "* " * i;
    print(leftStars + spaces + rightStars);
  }
  for (int i = n; i >= 1; i--) {
    String leftStars = "* " * i;
    String spaces = "  " * (2 * (n - i));
    String rightStars = "* " * i;
    print(leftStars + spaces + rightStars);
  }
}
```
**Output:**
```
*             * 
* *         * * 
* * *     * * * 
* * * * * * * * 
* * * * * * * * 
* * *     * * * 
* *         * * 
*             * 
```
**Complexity Analysis:**
- **Time Complexity:** $O(N^2)$ - Loop multiplication steps.
- **Space Complexity:** $O(N)$ - Printing buffer.

---

### 3.7 Diamond pattern
**Dart Code:**
```dart
void main() {
  printDiamond(4);
}

void printDiamond(int n) {
  for (int i = 1; i <= n; i++) {
    String spaces = " " * (n - i);
    String stars = "* " * i;
    print(spaces + stars);
  }
  for (int i = n - 1; i >= 1; i--) {
    String spaces = " " * (n - i);
    String stars = "* " * i;
    print(spaces + stars);
  }
}
```
**Output:**
```
   * 
  * * 
 * * * 
* * * * 
 * * * 
  * * 
   * 
```
**Complexity Analysis:**
- **Time Complexity:** $O(N^2)$ - Rows and spacing operations.
- **Space Complexity:** $O(N)$ - String memory allocation.

---

## 4. Maths & Logic

---

### 4.1 Check if a number is prime
**Dart Code:**
```dart
void main() {
  print("Is 7 prime? ${isPrime(7)}");   // Output: true
  print("Is 12 prime? ${isPrime(12)}"); // Output: false
}

bool isPrime(int n) {
  if (n <= 1) return false;
  
  for (int i = 2; i * i <= n; i++) {
    if (n % i == 0) {
      return false;
    }
  }
  return true;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(\sqrt{N})$ - We only iterate up to $\sqrt{N}$.
- **Space Complexity:** $O(1)$ - Constant workspace variables.

**Algorithmic Logic:**
A number $N$ is prime if it has no positive divisors other than $1$ and itself. If a factor exists, one of the factors must be less than or equal to $\sqrt{N}$. We check for division using the modulo operator up to this threshold.

---

### 4.2 Find GCD / HCF and LCM
**Dart Code:**
```dart
void main() {
  int num1 = 12;
  int num2 = 18;
  int gcdVal = findGCD(num1, num2);
  int lcmVal = findLCM(num1, num2, gcdVal);
  
  print("GCD: $gcdVal"); // Output: 6
  print("LCM: $lcmVal"); // Output: 36
}

int findGCD(int a, int b) {
  while (b != 0) {
    int temp = b;
    b = a % b;
    a = temp;
  }
  return a;
}

int findLCM(int a, int b, int gcd) {
  return (a * b) ~/ gcd;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(\log(\min(A, B)))$ - Euclidean algorithm runs in logarithmic time.
- **Space Complexity:** $O(1)$ - Constant space.

**Algorithmic Logic:**
- **GCD (Greatest Common Divisor):** We use the Euclidean algorithm which repeatedly replaces $A$ and $B$ with $B$ and $A \pmod B$ respectively until $B$ becomes $0$. The last non-zero value is the GCD.
- **LCM (Least Common Multiple):** Based on the algebraic relation $A \times B = \text{GCD}(A, B) \times \text{LCM}(A, B)$, we calculate $\text{LCM} = \frac{A \times B}{\text{GCD}}$.

---

### 4.3 Check if a number is palindrome
**Dart Code:**
```dart
void main() {
  print("Is 121 palindrome? ${isNumPalindrome(121)}"); // Output: true
}

bool isNumPalindrome(int n) {
  if (n < 0) return false;
  
  int original = n;
  int reversed = 0;
  
  while (n > 0) {
    int lastDigit = n % 10;
    reversed = (reversed * 10) + lastDigit;
    n = n ~/ 10;
  }
  
  return original == reversed;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(\log_{10} N)$ - Number of operations proportional to number of digits in $N$.
- **Space Complexity:** $O(1)$ - In-place digit reversal.

**Algorithmic Logic:**
We reverse the digits of the integer mathematically. By using modular arithmetic (`% 10`), we pull the trailing digit, and by integer division (`~/ 10`), we truncate it from the original number. Finally, we verify if the reversed value matches the original input.

---

### 4.4 Armstrong number
**Dart Code:**
```dart
import 'dart:math';

void main() {
  print("Is 153 Armstrong? ${isArmstrong(153)}"); // Output: true (1^3 + 5^3 + 3^3 = 153)
}

bool isArmstrong(int n) {
  int original = n;
  int sum = 0;
  int digitsCount = n.toString().length;
  
  while (n > 0) {
    int digit = n % 10;
    sum += pow(digit, digitsCount).toInt();
    n = n ~/ 10;
  }
  
  return sum == original;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(\log_{10} N)$ - Proportional to number of digits in $N$.
- **Space Complexity:** $O(1)$ - Storing temporary sum values.

**Algorithmic Logic:**
An Armstrong number is equal to the sum of its own digits raised to the power of the number of digits. We extract each digit sequentially, raise it to the exponent (number of digits), accumulate the results, and compare the sum to the original integer.

---

### 4.5 Fibonacci series
**Dart Code:**
```dart
void main() {
  printFibonacci(8); // Output: 0 1 1 2 3 5 8 13
}

void printFibonacci(int terms) {
  int first = 0, second = 1;
  String result = "";
  
  for (int i = 0; i < terms; i++) {
    result += "$first ";
    int next = first + second;
    first = second;
    second = next;
  }
  print(result);
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Linear addition sequences.
- **Space Complexity:** $O(1)$ - Only stores current and next terms.

**Algorithmic Logic:**
The sequence starts with $F_0 = 0, F_1 = 1$. Each subsequent element is computed dynamically by adding the two previous numbers ($F_n = F_{n-1} + F_{n-2}$).

---

### 4.6 Count digits in a number
**Dart Code:**
```dart
void main() {
  print("Digits: ${countDigits(5234)}"); // Output: 4
}

int countDigits(int n) {
  if (n == 0) return 1;
  int count = 0;
  while (n > 0) {
    count++;
    n = n ~/ 10;
  }
  return count;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(\log_{10} N)$ - Linear with respect to the digit count.
- **Space Complexity:** $O(1)$ - Simple integer counter.

**Algorithmic Logic:**
We systematically strip off the rightmost digit of the number using integer division (`~/ 10`) until the number reaches `0`. We keep a running counter to track how many times this operation is executed.

---

### 4.7 Reverse a number
**Dart Code:**
```dart
void main() {
  print("Reversed: ${reverseNumber(523)}"); // Output: 325
}

int reverseNumber(int n) {
  int reversed = 0;
  while (n > 0) {
    int lastDigit = n % 10;
    reversed = (reversed * 10) + lastDigit;
    n = n ~/ 10;
  }
  return reversed;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(\log_{10} N)$ - Logarithmic time complexity.
- **Space Complexity:** $O(1)$ - Constant storage.

**Algorithmic Logic:**
We extract the last digit of the number using `% 10`, append it to the `reversed` value ($reversed \times 10 + digit$), and drop the last digit of the original number with `~/ 10`.

---

## 5. Recursion

---

### 5.1 Factorial of a number
**Dart Code:**
```dart
void main() {
  print("Factorial of 5: ${factorial(5)}"); // Output: 120
}

int factorial(int n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Linear recursion depth of $N$.
- **Space Complexity:** $O(N)$ - Space on call stack.

**Algorithmic Logic:**
We define the recurrence relation: $F(N) = N \times F(N-1)$. The recursion breaks and begins unwinding when it encounters the base case: $N \le 1$, returning $1$.

---

### 5.2 Fibonacci series (Recursive)
**Dart Code:**
```dart
void main() {
  print("6th Fibonacci term: ${fibonacci(6)}"); // Output: 8
}

int fibonacci(int n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(2^N)$ - Exponential call tree.
- **Space Complexity:** $O(N)$ - Recursion call stack height.

**Algorithmic Logic:**
Calculates the N-th Fibonacci number through branching recursive calls: $F(N) = F(N-1) + F(N-2)$. The base cases are defined at $N = 0$ (returns 0) and $N = 1$ (returns 1).

---

### 5.3 Check if a string is palindrome (Recursive)
**Dart Code:**
```dart
void main() {
  String word = "radar";
  print("Is radar palindrome? ${isPalindromeRec(word, 0, word.length - 1)}"); // Output: true
}

bool isPalindromeRec(String s, int start, int end) {
  if (start >= end) return true;
  if (s[start] != s[end]) return false;
  return isPalindromeRec(s, start + 1, end - 1);
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Compares half of the characters.
- **Space Complexity:** $O(N)$ - Recursion frame stack.

**Algorithmic Logic:**
We recursively verify string symmetry. In each step, we compare characters at the index pointers `start` and `end`. If they match, we call the function recursively on the internal substring, shifting the boundary pointers inward. The base case is reached when `start >= end`.

---

### 5.4 Print all subsequences of a string
**Dart Code:**
```dart
void main() {
  printSubsequences("abc", "", 0);
}

void printSubsequences(String s, String current, int index) {
  if (index == s.length) {
    print(current == "" ? "{empty}" : current);
    return;
  }
  
  // Choice 1: Include current character
  printSubsequences(s, current + s[index], index + 1);
  
  // Choice 2: Exclude current character
  printSubsequences(s, current, index + 1);
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(2^N)$ - Double recursive branching at each element.
- **Space Complexity:** $O(N)$ - Maximum call stack depth.

**Algorithmic Logic:**
This implements a decision-tree structure. At each index, we branch into two choices: either include the character at `index` in the `current` subsequence or exclude it. When `index` equals the string length, we output the accumulated subsequence.

---

### 5.5 Tower of Hanoi
**Dart Code:**
```dart
void main() {
  towerOfHanoi(3, 'A', 'C', 'B'); // 3 disks
}

void towerOfHanoi(int n, String source, String destination, String helper) {
  if (n == 1) {
    print("Move disk 1 from $source to $destination");
    return;
  }
  towerOfHanoi(n - 1, source, helper, destination);
  print("Move disk $n from $source to $destination");
  towerOfHanoi(n - 1, helper, destination, source);
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(2^N)$ - Exponential move operations.
- **Space Complexity:** $O(N)$ - Stack recursion depth.

**Algorithmic Logic:**
To move $N$ disks from a source rod to a destination rod using an auxiliary (helper) rod:
1. Recursively move $N-1$ disks from `source` to `helper`.
2. Move the remaining largest disk directly from `source` to `destination`.
3. Recursively move the $N-1$ disks from `helper` to `destination`.

---

### 5.6 Sum of digits of a number (Recursive)
**Dart Code:**
```dart
void main() {
  print("Sum: ${sumOfDigits(254)}"); // Output: 11 (2 + 5 + 4)
}

int sumOfDigits(int n) {
  if (n == 0) return 0;
  return (n % 10) + sumOfDigits(n ~/ 10);
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(\log_{10} N)$ - Proportional to number of digits in $N$.
- **Space Complexity:** $O(\log_{10} N)$ - Call stack allocation.

**Algorithmic Logic:**
We calculate the sum recursively by extracting the last digit of the number using `% 10` and adding it to the result of the recursive call on the truncated number (`n ~/ 10`). The base case returns `0` when the number is completely reduced.

---

## 6. Basic Data Structures

---

### 6.1 Implement Stack using array
**Dart Code:**
```dart
void main() {
  Stack stack = Stack();
  stack.push(10);
  stack.push(20);
  print(stack.pop()); // Output: 20
  print(stack.peek()); // Output: 10
}

class Stack {
  final List<int> _storage = [];
  
  void push(int value) {
    _storage.add(value);
  }
  
  int pop() {
    if (isEmpty) throw StateError("Stack underflow!");
    return _storage.removeLast();
  }
  
  int peek() {
    if (isEmpty) throw StateError("Stack is empty!");
    return _storage.last;
  }
  
  bool get isEmpty => _storage.isEmpty;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(1)$ amortized for both push and pop.
- **Space Complexity:** $O(N)$ - Storage allocations.

**Algorithmic Logic:**
A Stack follows LIFO (Last-In, First-Out). In Dart, we implement this behavior on top of a dynamic `List` array using the built-in operations `add()` to append to the end (top) and `removeLast()` to pop the top element.

---

### 6.2 Implement Queue using array
**Dart Code:**
```dart
void main() {
  Queue queue = Queue();
  queue.enqueue(10);
  queue.enqueue(20);
  print(queue.dequeue()); // Output: 10
}

class Queue {
  final List<int> _storage = [];
  
  void enqueue(int value) {
    _storage.add(value);
  }
  
  int dequeue() {
    if (isEmpty) throw StateError("Queue underflow!");
    return _storage.removeAt(0);
  }
  
  bool get isEmpty => _storage.isEmpty;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(1)$ for enqueue, $O(N)$ for dequeue due to index shifting in list storage.
- **Space Complexity:** $O(N)$ - Memory capacity.

**Algorithmic Logic:**
A Queue implements FIFO (First-In, First-Out). We insert (enqueue) at the end of the array using `add()`, and we remove (dequeue) from the beginning using `removeAt(0)`.

---

### 6.3 Check for balanced parentheses
**Dart Code:**
```dart
void main() {
  print("Is {()} balanced? ${isBalanced('{()}')}"); // Output: true
}

bool isBalanced(String expression) {
  List<String> stack = [];
  Map<String, String> pairs = {')': '(', '}': '{', ']': '['};
  
  for (int i = 0; i < expression.length; i++) {
    String char = expression[i];
    
    if (char == '(' || char == '{' || char == '[') {
      stack.add(char);
    } else if (char == ')' || char == '}' || char == ']') {
      if (stack.isEmpty || stack.removeLast() != pairs[char]) {
        return false;
      }
    }
  }
  return stack.isEmpty;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Single scan of string characters.
- **Space Complexity:** $O(N)$ - Auxiliary stack tracking open brackets.

**Algorithmic Logic:**
We traverse the expression character-by-character. If we encounter an opening bracket (`(`, `{`, `[`), we push it onto our stack. When we encounter a closing bracket (`)`, `}`, `]`), we pop the top element from the stack and verify if it matches the corresponding opening bracket. If it doesn't match, or if the stack is empty when popped, the expression is unbalanced.

---

### 6.4 Infix to Postfix conversion (Shunting-Yard)
**Dart Code:**
```dart
void main() {
  print("Postfix: ${infixToPostfix('A+B*C')}"); // Output: ABC*+
}

int getPrecedence(String char) {
  if (char == '+' || char == '-') return 1;
  if (char == '*' || char == '/') return 2;
  return -1;
}

String infixToPostfix(String exp) {
  String result = "";
  List<String> stack = [];
  
  for (int i = 0; i < exp.length; i++) {
    String char = exp[i];
    
    if (RegExp(r'[a-zA-Z0-9]').hasMatch(char)) {
      result += char;
    } else if (char == '(') {
      stack.add(char);
    } else if (char == ')') {
      while (stack.isNotEmpty && stack.last != '(') {
        result += stack.removeLast();
      }
      stack.removeLast();
    } else {
      while (stack.isNotEmpty && getPrecedence(char) <= getPrecedence(stack.last)) {
        result += stack.removeLast();
      }
      stack.add(char);
    }
  }
  
  while (stack.isNotEmpty) {
    result += stack.removeLast();
  }
  return result;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Linear operations.
- **Space Complexity:** $O(N)$ - Auxiliary operator stack.

**Algorithmic Logic:**
We process the expression left-to-right using Dijkstra's Shunting-Yard algorithm. Operands are immediately output, while operators are stored in a stack. When an operator is processed, we pop operators of higher or equal precedence from the stack to the output before pushing the current operator. Parentheses define scope and override precedence rules.

---

### 6.5 Evaluate Postfix expression
**Dart Code:**
```dart
void main() {
  print("Result: ${evaluatePostfix('231*+9-')}"); // Output: -4
}

int evaluatePostfix(String exp) {
  List<int> stack = [];
  
  for (int i = 0; i < exp.length; i++) {
    String char = exp[i];
    
    if (RegExp(r'[0-9]').hasMatch(char)) {
      stack.add(int.parse(char));
    } else {
      int num2 = stack.removeLast();
      int num1 = stack.removeLast();
      
      switch (char) {
        case '+': stack.add(num1 + num2); break;
        case '-': stack.add(num1 - num2); break;
        case '*': stack.add(num1 * num2); break;
        case '/': stack.add(num1 ~/ num2); break;
      }
    }
  }
  return stack.removeLast();
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Single linear scan of expression.
- **Space Complexity:** $O(N)$ - Operand stack storage.

**Algorithmic Logic:**
We scan the postfix expression left-to-right. We push operands onto a stack. When an operator is encountered, we pop the top two operands from the stack, perform the respective mathematical calculation, and push the result back onto the stack.

---

## 7. Searching & Sorting

---

### 7.1 Binary Search
**Dart Code:**
```dart
void main() {
  List<int> numbers = [2, 5, 8, 12, 16, 23, 38, 56, 72];
  print("Index: ${binarySearch(numbers, 23)}"); // Output: 5
}

int binarySearch(List<int> sortedList, int target) {
  int low = 0;
  int high = sortedList.length - 1;
  
  while (low <= high) {
    int mid = low + ((high - low) ~/ 2);
    
    if (sortedList[mid] == target) {
      return mid;
    } else if (sortedList[mid] < target) {
      low = mid + 1;
    } else {
      high = mid - 1;
    }
  }
  return -1;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(\log N)$ - Reduces search space by half each step.
- **Space Complexity:** $O(1)$ - Iterative pointers.

**Algorithmic Logic:**
In a sorted array, we define high and low boundaries. We calculate the middle index `mid`. If the middle element matches our target, we return its index. If target is larger than the middle element, we adjust the `low` pointer. Otherwise, we adjust the `high` pointer.

---

### 7.2 Linear Search
**Dart Code:**
```dart
void main() {
  List<int> numbers = [5, 3, 8, 2, 9];
  print("Index: ${linearSearch(numbers, 2)}"); // Output: 3
}

int linearSearch(List<int> list, int target) {
  for (int i = 0; i < list.length; i++) {
    if (list[i] == target) {
      return i;
    }
  }
  return -1;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Checks each element in sequence.
- **Space Complexity:** $O(1)$ - No helper storage.

**Algorithmic Logic:**
We iterate sequentially through the elements of the list, comparing each element with the target value. We return the index if a match is found; otherwise, we return `-1`.

---

### 7.3 Bubble Sort
**Dart Code:**
```dart
void main() {
  List<int> numbers = [64, 34, 25, 12, 22, 11, 90];
  bubbleSort(numbers);
  print("Sorted array: $numbers"); // Output: [11, 12, 22, 25, 34, 64, 90]
}

void bubbleSort(List<int> list) {
  int n = list.length;
  for (int i = 0; i < n - 1; i++) {
    for (int j = 0; j < n - i - 1; j++) {
      if (list[j] > list[j + 1]) {
        int temp = list[j];
        list[j] = list[j + 1];
        list[j + 1] = temp;
      }
    }
  }
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N^2)$ - Average and worst-case comparison passes.
- **Space Complexity:** $O(1)$ - Constant space in-place sorting.

**Algorithmic Logic:**
We iteratively traverse the array from start to end, comparing adjacent elements. If the left element is larger than the right element, they are swapped. In each full pass, the largest unsorted element is placed at its correct sorted index.

---

### 7.4 Selection Sort
**Dart Code:**
```dart
void main() {
  List<int> numbers = [64, 25, 12, 22, 11];
  selectionSort(numbers);
  print("Sorted array: $numbers"); // Output: [11, 12, 22, 25, 64]
}

void selectionSort(List<int> list) {
  int n = list.length;
  for (int i = 0; i < n - 1; i++) {
    int minIndex = i;
    for (int j = i + 1; j < n; j++) {
      if (list[j] < list[minIndex]) {
        minIndex = j;
      }
    }
    int temp = list[minIndex];
    list[minIndex] = list[i];
    list[i] = temp;
  }
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N^2)$ - Compares all elements to find minimum values.
- **Space Complexity:** $O(1)$ - In-place swaps.

**Algorithmic Logic:**
We divide the array into sorted and unsorted regions. In each iteration, we locate the minimum element in the unsorted region and swap it with the first element of the unsorted region, extending the sorted boundary by one.

---

### 7.5 Insertion Sort
**Dart Code:**
```dart
void main() {
  List<int> numbers = [12, 11, 13, 5, 6];
  insertionSort(numbers);
  print("Sorted array: $numbers"); // Output: [5, 6, 11, 12, 13]
}

void insertionSort(List<int> list) {
  for (int i = 1; i < list.length; i++) {
    int key = list[i];
    int j = i - 1;
    
    while (j >= 0 && list[j] > key) {
      list[j + 1] = list[j];
      j--;
    }
    list[j + 1] = key;
  }
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N^2)$ in worst case, $O(N)$ when array is already sorted.
- **Space Complexity:** $O(1)$ - In-place shifting.

**Algorithmic Logic:**
We traverse from index `1` to `N-1`. We store the current element as `key` and shift all sorted elements that are greater than `key` one position to their right. We then insert the `key` into its correct sorted location.

---

### 7.6 Merge Sort
**Dart Code:**
```dart
void main() {
  List<int> numbers = [38, 27, 43, 3, 9, 82, 10];
  print("Sorted: ${mergeSort(numbers)}"); // Output: [3, 9, 10, 27, 38, 43, 82]
}

List<int> mergeSort(List<int> list) {
  if (list.length <= 1) return list;
  
  int mid = list.length ~/ 2;
  List<int> left = mergeSort(list.sublist(0, mid));
  List<int> right = mergeSort(list.sublist(mid));
  
  return merge(left, right);
}

List<int> merge(List<int> left, List<int> right) {
  List<int> result = [];
  int i = 0, j = 0;
  
  while (i < left.length && j < right.length) {
    if (left[i] < right[j]) {
      result.add(left[i]);
      i++;
    } else {
      result.add(right[j]);
      j++;
    }
  }
  
  result.addAll(left.sublist(i));
  result.addAll(right.sublist(j));
  return result;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N \log N)$ - Consistently splits and merges list structures.
- **Space Complexity:** $O(N)$ - Auxiliary arrays for merging.

**Algorithmic Logic:**
Merge Sort uses divide-and-conquer strategy:
1. Divide the unsorted array in half.
2. Recursively sort both sub-arrays.
3. Merge (combine) the two sorted sub-arrays back into a single sorted array by comparing front elements.

---

### 7.7 Quick Sort
**Dart Code:**
```dart
void main() {
  List<int> numbers = [10, 80, 30, 90, 40, 50, 70];
  quickSort(numbers, 0, numbers.length - 1);
  print("Sorted array: $numbers"); // Output: [10, 30, 40, 50, 70, 80, 90]
}

void quickSort(List<int> list, int low, int high) {
  if (low < high) {
    int pivotIndex = partition(list, low, high);
    quickSort(list, low, pivotIndex - 1);
    quickSort(list, pivotIndex + 1, high);
  }
}

int partition(List<int> list, int low, int high) {
  int pivot = list[high];
  int i = low - 1;
  
  for (int j = low; j < high; j++) {
    if (list[j] < pivot) {
      i++;
      int temp = list[i];
      list[i] = list[j];
      list[j] = temp;
    }
  }
  int temp = list[i + 1];
  list[i + 1] = list[high];
  list[high] = temp;
  
  return i + 1;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N \log N)$ average-case, $O(N^2)$ worst-case (if arrays are already sorted and pivot selection is poor).
- **Space Complexity:** $O(\log N)$ - Average height of call stack frames.

**Algorithmic Logic:**
We select an element as a pivot (e.g. the last element). We partition the array so that all elements smaller than the pivot are placed to its left, and all elements larger than the pivot are placed to its right. We then recursively apply Quick Sort on the left and right subarrays.

---

## 8. Important Problem Solving

---

### 8.1 Two Sum Problem
**Dart Code:**
```dart
void main() {
  List<int> numbers = [2, 7, 11, 15];
  print("Indices: ${twoSum(numbers, 9)}"); // Output: [0, 1]
}

List<int> twoSum(List<int> list, int target) {
  Map<int, int> numMap = {};
  
  for (int i = 0; i < list.length; i++) {
    int complement = target - list[i];
    if (numMap.containsKey(complement)) {
      return [numMap[complement]!, i];
    }
    numMap[list[i]] = i;
  }
  return [];
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Single scan with $O(1)$ hash lookups.
- **Space Complexity:** $O(N)$ - Storage for seen elements.

**Algorithmic Logic:**
We use a hash map to map values to their indices. For each element, we check if the difference ($target - current\_value$) is present in the map. If it is present, we return its index along with the current index. Otherwise, we add the current value and its index to the map.

---

### 8.2 Check if array contains a pair with given sum
**Dart Code:**
```dart
void main() {
  List<int> numbers = [1, 4, 45, 6, 10, 8];
  print("Has pair? ${hasPairWithSum(numbers, 16)}"); // Output: true
}

bool hasPairWithSum(List<int> list, int sum) {
  Set<int> seen = {};
  for (int num in list) {
    if (seen.contains(sum - num)) {
      return true;
    }
    seen.add(num);
  }
  return false;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Single pass hash lookup.
- **Space Complexity:** $O(N)$ - Storing processed elements.

**Algorithmic Logic:**
We iterate through the array, storing visited elements in a hash set. For each element, we check if the target difference ($sum - current\_element$) has already been registered in our set. If so, a pair exists.

---

### 8.3 Find majority element (Boyer-Moore Voting)
**Dart Code:**
```dart
void main() {
  List<int> numbers = [2, 2, 1, 1, 1, 2, 2];
  print("Majority element: ${findMajority(numbers)}"); // Output: 2
}

int? findMajority(List<int> list) {
  int? candidate;
  int count = 0;
  
  for (int num in list) {
    if (count == 0) {
      candidate = num;
      count = 1;
    } else if (num == candidate) {
      count++;
    } else {
      count--;
    }
  }
  
  int verificationCount = 0;
  for (int num in list) {
    if (num == candidate) verificationCount++;
  }
  
  return (verificationCount > list.length / 2) ? candidate : null;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Two linear sweeps.
- **Space Complexity:** $O(1)$ - Constant space variables.

**Algorithmic Logic:**
Using the Boyer-Moore Voting Algorithm, we keep a `candidate` and a `count` variable. We traverse the array; if the count reaches $0$, we set the current element as the candidate and set the count to $1$. If the current element matches the candidate, we increment count; otherwise, we decrement count. Finally, we verify that the candidate appears more than $\lfloor N/2 \rfloor$ times.

---

### 8.4 Leaders in an array
**Dart Code:**
```dart
void main() {
  List<int> numbers = [16, 17, 4, 3, 5, 2];
  print("Leaders: ${findLeaders(numbers)}"); // Output: [17, 5, 2]
}

List<int> findLeaders(List<int> list) {
  List<int> leaders = [];
  if (list.isEmpty) return leaders;
  
  int maxFromRight = list.last;
  leaders.add(maxFromRight);
  
  for (int i = list.length - 2; i >= 0; i--) {
    if (list[i] > maxFromRight) {
      maxFromRight = list[i];
      leaders.add(maxFromRight);
    }
  }
  return leaders.reversed.toList();
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Single backward sweep.
- **Space Complexity:** $O(1)$ - Constant variables, excluding space for output list.

**Algorithmic Logic:**
An element is a leader if it is greater than all elements to its right. We scan the array backward starting from the last element (which is always a leader) while maintaining the maximum element encountered so far. If the current element is greater than this maximum, it is identified as a leader.

---

### 8.5 Trapping Rain Water
**Dart Code:**
```dart
void main() {
  List<int> heights = [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1];
  print("Trapped water: ${trapWater(heights)}"); // Output: 6
}

int trapWater(List<int> heights) {
  if (heights.isEmpty) return 0;
  
  int left = 0;
  int right = heights.length - 1;
  int leftMax = 0;
  int rightMax = 0;
  int totalWater = 0;
  
  while (left < right) {
    if (heights[left] < heights[right]) {
      if (heights[left] >= leftMax) {
        leftMax = heights[left];
      } else {
        totalWater += leftMax - heights[left];
      }
      left++;
    } else {
      if (heights[right] >= rightMax) {
        rightMax = heights[right];
      } else {
        totalWater += rightMax - heights[right];
      }
      right--;
    }
  }
  return totalWater;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Single pass using two pointers.
- **Space Complexity:** $O(1)$ - Constant space variables.

**Algorithmic Logic:**
Using two pointers (`left` and `right`), we move inward. The volume of water trapped over any building is bounded by the lower of the maximum heights seen on the left and right sides. We evaluate the smaller of `heights[left]` and `heights[right]`, tracking `leftMax` and `rightMax` updates.

---

### 8.6 Maximum / Minimum in sliding window
**Dart Code:**
```dart
import 'dart:collection';

void main() {
  List<int> numbers = [1, 3, -1, -3, 5, 3, 6, 7];
  print("Max in sliding window (k=3): ${maxSlidingWindow(numbers, 3)}");
  // Output: [3, 3, 5, 5, 6, 7]
}

List<int> maxSlidingWindow(List<int> nums, int k) {
  List<int> result = [];
  if (nums.isEmpty || k <= 0) return result;
  
  DoubleLinkedQueue<int> deque = DoubleLinkedQueue<int>();
  
  for (int i = 0; i < nums.length; i++) {
    if (deque.isNotEmpty && deque.first == i - k) {
      deque.removeFirst();
    }
    
    while (deque.isNotEmpty && nums[deque.last] < nums[i]) {
      deque.removeLast();
    }
    
    deque.addLast(i);
    
    if (i >= k - 1) {
      result.add(nums[deque.first]);
    }
  }
  return result;
}
```
**Complexity Analysis:**
- **Time Complexity:** $O(N)$ - Every element is added and removed from the double-ended queue at most once.
- **Space Complexity:** $O(k)$ - The deque size is bounded by window size $k$.

**Algorithmic Logic:**
We use a monotonic double-ended queue (deque) to store indices of array elements. As we slide the window, we pop indices that fall outside the window's left boundary. We also pop indices from the back of the deque if their corresponding values are less than the incoming element, keeping the deque sorted in descending order of values. The front of the deque always represents the maximum element of the active window.
