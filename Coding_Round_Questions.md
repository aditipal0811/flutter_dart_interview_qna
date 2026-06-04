# Coding Round Questions & Answers (For Beginners & Kids!)

Welcome! Below you will find simple Dart solutions for all the coding round questions, along with child-friendly explanations. 

---

## 1. Arrays (Lists of Toys)

An **array** (called a `List` in Dart) is like a row of toy boxes. Each box has a number label (called an **index**), starting from `0`.

---

### 1.1 Find the second largest element in an array
**Dart Code:**
```dart
void main() {
  List<int> numbers = [12, 35, 1, 10, 34, 1];
  int? secondLargest = findSecondLargest(numbers);
  print("The second largest number is: $secondLargest"); // Output: 34
}

int? findSecondLargest(List<int> list) {
  if (list.length < 2) return null; // We need at least two toys to compare!

  int largest = -999999;
  int secondLargest = -999999;

  for (int number in list) {
    if (number > largest) {
      secondLargest = largest; // Old champion becomes second place
      largest = number;        // We have a new champion!
    } else if (number > secondLargest && number != largest) {
      secondLargest = number;  // Sneaks into second place
    }
  }

  return secondLargest == -999999 ? null : secondLargest;
}
```
**The Magic Analogy:**
Imagine you are a judge at a tall-building contest. You look at buildings one by one. You keep two sticky notes: "Tallest" and "Second Tallest". When you see a building taller than your "Tallest", you move the old tallest to "Second Tallest" and write the new building on the "Tallest" note.
- **Remember key:** The runner-up takes the old champion's crown when a new champion arrives!

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
      return false; // Uh-oh, we walked down a step!
    }
  }
  return true; // We only walked flat or upwards!
}
```
**The Magic Analogy:**
Imagine you are walking up a staircase. To go up, each step must be higher than or equal to the step you are standing on. If you ever find a step that goes *down*, the staircase is broken (not sorted)!
- **Remember key:** Compare each step with the one right after it. No stepping down allowed!

---

### 1.3 Remove duplicates from a sorted array
**Dart Code:**
```dart
void main() {
  List<int> numbers = [1, 1, 2, 2, 3, 4, 4];
  int newLength = removeDuplicates(numbers);
  print("Cleaned list: ${numbers.sublist(0, newLength)}"); // Output: [1, 2, 3, 4]
}

int removeDuplicates(List<int> list) {
  if (list.isEmpty) return 0;
  
  int uniqueIndex = 0; // Where the next unique toy goes
  
  for (int i = 1; i < list.length; i++) {
    if (list[i] != list[uniqueIndex]) {
      uniqueIndex++;
      list[uniqueIndex] = list[i]; // Move the unique toy forward
    }
  }
  return uniqueIndex + 1; // Number of unique toys
}
```
**The Magic Analogy:**
You have a line of kids wearing numbered shirts. Some shirts are duplicates. Since they are sorted, duplicates stand right next to each other. You walk down the line, and whenever you see a kid with a *new* number, you pull them forward to form a clean, duplicate-free line at the front!
- **Remember key:** Use two markers—one to find new numbers, and one to place them in their new home.

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
  d = d % n; // If we rotate a circle fully, it's back to normal!
  
  // Magic trick: reverse sections!
  reverse(list, 0, d - 1);
  reverse(list, d, n - 1);
  reverse(list, 0, n - 1);
  return list;
}

List<int> rotateRight(List<int> list, int d) {
  int n = list.length;
  d = d % n;
  
  // Magic trick: reverse sections!
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
**The Magic Analogy:**
Imagine a toy train on a circular track. Left rotation by 2 means the first 2 cars get unhooked and moved to the very back of the train. The reversing trick is like flipping the first part, flipping the second part, and then flipping the whole train to magically put them in the right order!
- **Remember key:** Reversing sections is a shortcut to rotating lists without using extra boxes.

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
  int nonZeroPos = 0; // Where the next non-zero number should sit
  
  for (int i = 0; i < list.length; i++) {
    if (list[i] != 0) {
      // Swap the non-zero element with the placeholder
      int temp = list[i];
      list[i] = list[nonZeroPos];
      list[nonZeroPos] = temp;
      nonZeroPos++;
    }
  }
}
```
**The Magic Analogy:**
Imagine you have a row of boxes. Some have toys (numbers) and some are empty (zeros). You walk along the row. Whenever you see a box with a toy, you swap it with the leftmost empty box you left behind. This pushes all the empty boxes to the end!
- **Remember key:** Drag all non-zero numbers to the front, and the zeros will automatically get pushed to the back.

---

### 1.6 Find the missing number in an array of 1 to N
**Dart Code:**
```dart
void main() {
  List<int> numbers = [1, 2, 4, 5, 6]; // N is 6, but 3 is missing!
  int missing = findMissingNumber(numbers, 6);
  print("The missing number is: $missing"); // Output: 3
}

int findMissingNumber(List<int> list, int n) {
  // Magic math formula for sum of 1 to N
  int expectedSum = (n * (n + 1)) ~/ 2; 
  
  int actualSum = 0;
  for (int num in list) {
    actualSum += num;
  }
  
  return expectedSum - actualSum;
}
```
**The Magic Analogy:**
Imagine you have a box of 10 crayons, but one is missing. If you know that a full box of crayons always sums up to 55 (1+2+3...+10), you can just add up the crayons you *do* have. If they sum up to 52, you know crayon number 3 (55 - 52) is the one lost under the sofa!
- **Remember key:** `Expected Sum - Actual Sum = Missing Number`.

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
  Set<int> unionSet = {}; // A Set never holds duplicates!
  unionSet.addAll(a);
  unionSet.addAll(b);
  return unionSet.toList();
}

List<int> findIntersection(List<int> a, List<int> b) {
  Set<int> setA = a.toSet();
  Set<int> intersectionSet = {};
  
  for (int item in b) {
    if (setA.contains(item)) {
      intersectionSet.add(item); // Only items present in both!
    }
  }
  return intersectionSet.toList();
}
```
**The Magic Analogy:**
- **Union:** You and your friend dump all your toys into one big chest. If you both have the same toy, you only keep one.
- **Intersection:** You check which toys you *both* own. If you both have a toy, you put it in the "Common Toys" box.
- **Remember key:** Sets are magic collections that ignore duplicates. Use them for Union and Intersection!

---

### 1.8 Kadane's Algorithm (Maximum Subarray Sum)
**Dart Code:**
```dart
void main() {
  List<int> numbers = [-2, 1, -3, 4, -1, 2, 1, -5, 4];
  print("Max contiguous subarray sum: ${maxSubarraySum(numbers)}"); // Output: 6 (Subarray is [4, -1, 2, 1])
}

int maxSubarraySum(List<int> list) {
  if (list.isEmpty) return 0;
  
  int maxSoFar = list[0];
  int currentSum = list[0];
  
  for (int i = 1; i < list.length; i++) {
    // Should we add the next number to our streak, or start a new streak?
    currentSum = (list[i] > currentSum + list[i]) ? list[i] : currentSum + list[i];
    
    // Update our record-holder
    if (currentSum > maxSoFar) {
      maxSoFar = currentSum;
    }
  }
  return maxSoFar;
}
```
**The Magic Analogy:**
Imagine you are running a course collecting coins (positive numbers) and stepping in mud pits that lose you coins (negative numbers). If you lose so many coins that your pocket is empty (your sum drops below zero), you throw away all history and start collecting coins fresh from the very next step!
- **Remember key:** If your current streak becomes worse than starting fresh, reset the streak!

---

## 2. Strings (Rows of Letters)

A **string** is like a word spelled with letter blocks placed side-by-side.

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
**The Magic Analogy:**
Imagine a row of letter blocks. You swap the first block with the last block, then the second block with the second-to-last block, and keep moving inward until you meet in the middle. The word is now backward!
- **Remember key:** Swap outer letters and step inward until you cross paths.

---

### 2.2 Check if a string is a palindrome
**Dart Code:**
```dart
void main() {
  print("Is 'racecar' a palindrome? ${isPalindrome('racecar')}"); // Output: true
  print("Is 'hello' a palindrome? ${isPalindrome('hello')}");     // Output: false
}

bool isPalindrome(String input) {
  int start = 0;
  int end = input.length - 1;
  
  while (start < end) {
    if (input[start] != input[end]) {
      return false; // Letters don't match!
    }
    start++;
    end--;
  }
  return true;
}
```
**The Magic Analogy:**
A palindrome is a word that reads the same forward and backward, like "RACECAR". You read the first letter and the last letter. If they match, you check the next ones moving inward. If they all match, you have a palindrome!
- **Remember key:** Compare start and end, moving inward.

---

### 2.3 Check if two strings are anagrams
**Dart Code:**
```dart
void main() {
  print("Are 'silent' and 'listen' anagrams? ${isAnagram('silent', 'listen')}"); // Output: true
}

bool isAnagram(String a, String b) {
  if (a.length != b.length) return false;
  
  // Sort the characters of both words
  List<String> listA = a.split('')..sort();
  List<String> listB = b.split('')..sort();
  
  // Compare the sorted lists
  for (int i = 0; i < listA.length; i++) {
    if (listA[i] != listB[i]) return false;
  }
  return true;
}
```
**The Magic Analogy:**
Imagine you have two piles of letter blocks. If you rearrange both piles alphabetically (sorting them), they should look exactly the same if they were made of the same letters! For example, "silent" and "listen" both sort to "eilnst".
- **Remember key:** `Sort and Compare`. If they are anagrams, their sorted letters are identical.

---

### 2.4 Find the first non-repeating character
**Dart Code:**
```dart
void main() {
  String word = "swiss";
  print("First non-repeating letter: ${firstNonRepeatingChar(word)}"); // Output: 'w'
}

String? firstNonRepeatingChar(String text) {
  Map<String, int> letterCounts = {};
  
  // Count how many times we see each letter
  for (int i = 0; i < text.length; i++) {
    String char = text[i];
    letterCounts[char] = (letterCounts[char] ?? 0) + 1;
  }
  
  // Find the first letter in the string that has a count of 1
  for (int i = 0; i < text.length; i++) {
    String char = text[i];
    if (letterCounts[char] == 1) {
      return char;
    }
  }
  return null;
}
```
**The Magic Analogy:**
Imagine walking through a classroom of kids. You write down how many times you see each name on a tally board. Then, you walk down the line again from start to end, checking your board. The first name you point to that only has 1 tally mark is your winner!
- **Remember key:** Count first, then read from the start to find the first single-count character.

---

### 2.5 Remove duplicate characters from a string
**Dart Code:**
```dart
void main() {
  print("Clean: ${removeDuplicateChars('programming')}"); // Output: "progamin"
}

String removeDuplicateChars(String input) {
  Set<String> seen = {};
  List<String> result = [];
  
  for (int i = 0; i < input.length; i++) {
    String char = input[i];
    if (!seen.contains(char)) {
      seen.add(char);
      result.add(char); // Save it if we haven't seen it yet!
    }
  }
  return result.join('');
}
```
**The Magic Analogy:**
You are collecting stamps. Every time you get a letter block, you check your collector's basket. If you don't have it, you add it to the display. If you already have it, you throw it away!
- **Remember key:** Keep a "Seen" set to check if you've run into this letter before.

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
    
    // Check if it's a letter first
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
**The Magic Analogy:**
You are sorting fruits. Vowels are red apples (A, E, I, O, U) and consonants are green apples. Spaces and punctuation are just leaves that you throw away. You count each type as you sort them.
- **Remember key:** Use a matching string or set of vowels to quickly check who belongs where.

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
  
  String prefix = words[0]; // Start by assuming the first word is the match
  
  for (int i = 1; i < words.length; i++) {
    // Shrink the prefix until it fits the current word
    while (!words[i].startsWith(prefix)) {
      prefix = prefix.substring(0, prefix.length - 1);
      if (prefix.isEmpty) return "";
    }
  }
  return prefix;
}
```
**The Magic Analogy:**
Imagine three friends: "flower", "flow", and "flight". You write "flower" on a chalkboard. You look at "flow" and erase letters from the end of "flower" until it matches: "flow". Then you look at "flight" and erase letters from the end of "flow" until it matches: "fl". What's left on the board is the common beginning!
- **Remember key:** Start with the first word, then chop off letters from the end until it fits all other words.

---

### 2.8 Implement atoi() / myAtoi() (String to Integer)
**Dart Code:**
```dart
void main() {
  print(myAtoi("   -42")); // Output: -42
  print(myAtoi("4193 with words")); // Output: 4193
}

int myAtoi(String s) {
  s = s.trim(); // 1. Clean spaces
  if (s.isEmpty) return 0;
  
  int sign = 1;
  int index = 0;
  
  // 2. Check for sign
  if (s[0] == '-') {
    sign = -1;
    index++;
  } else if (s[0] == '+') {
    index++;
  }
  
  double result = 0; // Using double to prevent overflow issues during conversion
  
  // 3. Read numbers
  while (index < s.length) {
    int codeUnit = s.codeUnitAt(index);
    // Is it a number character between '0' and '9'?
    if (codeUnit >= 48 && codeUnit <= 57) {
      int digit = codeUnit - 48; // Convert char to actual int
      result = (result * 10) + digit;
    } else {
      break; // Stop at first non-digit!
    }
    index++;
  }
  
  double finalVal = result * sign;
  
  // 4. Handle boundaries (32-bit Integer limits)
  int minInt = -2147483648;
  int maxInt = 2147483647;
  if (finalVal < minInt) return minInt;
  if (finalVal > maxInt) return maxInt;
  
  return finalVal.toInt();
}
```
**The Magic Analogy:**
You want to read numbers from a text string. First, blow away any empty spaces. Next, check if it starts with a minus sign `-`. Then, read each number digit. To add a digit, multiply your current score by 10 (moving it to the left) and add the new digit!
- **Remember key:** `result = (result * 10) + digit`.

---

## 3. Patterns (Drawing with Loop Stamps)

We can draw stars `*` on the screen using loops. Think of loops as repeating stamp machines.

---

### 3.1 Print solid patterns
**Dart Code:**
```dart
// Solid Square of 4x4
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
**The Magic Analogy:**
Imagine you are tiling a floor. You go row by row, putting a tile in every column space until the grid is full!

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
      // Print star only if it is the boundary
      if (row == 0 || row == size - 1 || col == 0 || col == size - 1) {
        line += "* ";
      } else {
        line += "  "; // Empty space inside
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
**The Magic Analogy:**
Imagine building a picture frame. You only put wood pieces on the outer edges (first row, last row, first column, last column) and leave the inside open for the picture.

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
**The Magic Analogy:**
For each row, you start writing numbers starting from `1`, going up until you hit the row number you are currently standing on!

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
      count++; // Keep counting up!
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
**The Magic Analogy:**
You write numbers on blocks starting at `1`. For each step you go down, you add one more block to that row, keeping the numbers going up consecutively without ever resetting!

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
**The Magic Analogy:**
You are stacking blocks, starting with a wide base at the top. With each step down, you make the row smaller by one block until you end with just one block at the bottom!

---

### 3.6 Butterfly pattern
**Dart Code:**
```dart
void main() {
  printButterfly(4);
}

void printButterfly(int n) {
  // Upper half
  for (int i = 1; i <= n; i++) {
    String leftStars = "* " * i;
    String spaces = "  " * (2 * (n - i));
    String rightStars = "* " * i;
    print(leftStars + spaces + rightStars);
  }
  // Lower half
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
**The Magic Analogy:**
Draw left wing stars, add some empty sky space in the middle, then draw right wing stars. Repeat while widening the wings, and then mirror it upside down!

---

### 3.7 Diamond pattern
**Dart Code:**
```dart
void main() {
  printDiamond(4);
}

void printDiamond(int n) {
  // Upper Pyramid
  for (int i = 1; i <= n; i++) {
    String spaces = " " * (n - i);
    String stars = "* " * i;
    print(spaces + stars);
  }
  // Lower Inverted Pyramid
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
**The Magic Analogy:**
To print a diamond, you draw spaces to push the stars to the middle. Grow the rows of stars, then shrink them back down to a single point!

---

## 4. Maths & Logic (Math Riddles)

Let's solve fun math riddles with code.

---

### 4.1 Check if a number is prime
**Dart Code:**
```dart
void main() {
  print("Is 7 prime? ${isPrime(7)}");   // Output: true
  print("Is 12 prime? ${isPrime(12)}"); // Output: false
}

bool isPrime(int n) {
  if (n <= 1) return false; // 1 and under are not prime!
  
  // Check divisors from 2 up to the square root of n
  for (int i = 2; i * i <= n; i++) {
    if (n % i == 0) {
      return false; // Found a divider, not prime!
    }
  }
  return true; // No one can divide it except 1 and itself!
}
```
**The Magic Analogy:**
A prime number is a very "stubborn" group of candy blocks. If you have 7 candies, you can't divide them into equal piles for your friends unless you give all 7 to one person, or 1 candy to 7 people. You can't make neat piles of 2 or 3!
- **Remember key:** If any number from 2 to $\sqrt{N}$ can divide the number evenly (`% == 0`), it's not prime.

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
    b = a % b; // Keep taking remainder
    a = temp;
  }
  return a;
}

int findLCM(int a, int b, int gcd) {
  return (a * b) ~/ gcd; // Using GCD to find LCM easily!
}
```
**The Magic Analogy:**
- **GCD:** Imagine you have 12 blue cookies and 18 red cookies. The GCD is the biggest size of cookie plates you can use to distribute both kinds of cookies perfectly evenly with zero leftovers! (Here, 6 plates).
- **LCM:** If one toy train completes a track loop in 12 seconds and another in 18 seconds, the LCM is the exact second they will cross the starting line together again! (36 seconds).
- **Remember key:** `GCD` can be found by continually dividing the numbers (Euclidean algorithm). `LCM = (A * B) / GCD`.

---

### 4.3 Check if a number is palindrome
**Dart Code:**
```dart
void main() {
  print("Is 121 palindrome? ${isNumPalindrome(121)}"); // Output: true
}

bool isNumPalindrome(int n) {
  if (n < 0) return false; // Negative numbers are not palindromes!
  
  int original = n;
  int reversed = 0;
  
  while (n > 0) {
    int lastDigit = n % 10;
    reversed = (reversed * 10) + lastDigit;
    n = n ~/ 10; // Cut off the last digit
  }
  
  return original == reversed;
}
```
**The Magic Analogy:**
You have a number written on blocks: `121`. You pull off the last block (`1`) and put it first, then pull the next block (`2`), building a new number backwards. If the backwards number is exactly the same as the original, it's a palindrome!
- **Remember key:** Reverse the number using math (`~/ 10` and `% 10`) and compare with the starting number.

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
**The Magic Analogy:**
An Armstrong number is a magic number whose digits are super-powered! If you take each digit and multiply it by itself as many times as the number has digits (cubed for a 3-digit number like $1^3 + 5^3 + 3^3$) and add them up, they magically build the original number!
- **Remember key:** `Sum of (each digit ^ total digits) = original number`.

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
**The Magic Analogy:**
Imagine breeding magic rabbits. The first month you have 0, the second 1. From then on, the number of rabbits you have is the sum of the rabbits you had in the last two months combined (0, 1, 1, 2, 3, 5, 8...).
- **Remember key:** `next = current + previous`.

---

### 4.6 Count digits in a number
**Dart Code:**
```dart
void main() {
  print("Digits in 5234: ${countDigits(5234)}"); // Output: 4
}

int countDigits(int n) {
  if (n == 0) return 1;
  int count = 0;
  while (n > 0) {
    count++;
    n = n ~/ 10; // Erase the last digit
  }
  return count;
}
```
**The Magic Analogy:**
Imagine you have a stack of dollar bills. Each time you divide by 10, you remove the top bill from the stack. You count how many times you do this until the stack is completely empty!
- **Remember key:** Keep dividing by 10 (`~/ 10`) until the number hits 0.

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
**The Magic Analogy:**
To reverse `523`, peel off the last digit `3` (using `% 10`) and start a new number. Then peel off the next digit `2`, multiply the previous part by 10 (`30 + 2 = 32`), and then the last digit `5` (`320 + 5 = 325`).
- **Remember key:** Use `% 10` to get the last digit, and `~/ 10` to throw it away.

---

## 5. Recursion (Nesting Dolls)

**Recursion** is when a function calls itself. It is like a Russian nesting doll: inside a big doll is a slightly smaller doll, which contains an even smaller one, until you reach the smallest doll that won't open (called the **base case**).

---

### 5.1 Factorial of a number
**Dart Code:**
```dart
void main() {
  print("Factorial of 5: ${factorial(5)}"); // Output: 120 (5 * 4 * 3 * 2 * 1)
}

int factorial(int n) {
  if (n <= 1) return 1; // Base case: Smallest doll!
  return n * factorial(n - 1); // Recursive call
}
```
**The Magic Analogy:**
To find the factorial of 5, you ask your friend: "Hey, what is the factorial of 4? I will multiply it by 5." Your friend asks their friend for the factorial of 3, all the way down to the kid asked for the factorial of 1, who replies immediately: "It's 1!". The answer then travels back up.
- **Remember key:** `factorial(n) = n * factorial(n - 1)`. Base case is `n <= 1`.

---

### 5.2 Fibonacci series (Recursive)
**Dart Code:**
```dart
void main() {
  print("6th Fibonacci term: ${fibonacci(6)}"); // Output: 8
}

int fibonacci(int n) {
  if (n <= 1) return n; // Base case
  return fibonacci(n - 1) + fibonacci(n - 2);
}
```
**The Magic Analogy:**
To find the height of a house, you add the height of the ground floor and the first floor. To find the height of those floors, you ask the smaller sub-floors, all the way down to floor 0 and floor 1 which you already know.
- **Remember key:** Sum the result of the two previous steps: `fib(n-1) + fib(n-2)`.

---

### 5.3 Check if a string is palindrome (Recursive)
**Dart Code:**
```dart
void main() {
  String word = "radar";
  print("Is radar palindrome? ${isPalindromeRec(word, 0, word.length - 1)}"); // Output: true
}

bool isPalindromeRec(String s, int start, int end) {
  if (start >= end) return true; // Base case: Middle reached!
  if (s[start] != s[end]) return false; // Outer mismatch!
  return isPalindromeRec(s, start + 1, end - 1); // Check inside
}
```
**The Magic Analogy:**
Look at the outer letters of "RADAR". 'R' and 'R' match. Peel them off! Now look at the inner part "ADA". Check 'A' and 'A'. Match! Peel them off. Now check 'D'. Since it is just 1 letter left, you are done!
- **Remember key:** Check outer letters, then recurse on the smaller inner word.

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
  
  // Option 1: Choose to include the character
  printSubsequences(s, current + s[index], index + 1);
  
  // Option 2: Choose NOT to include the character
  printSubsequences(s, current, index + 1);
}
```
**Output:**
```
abc
ab
ac
a
bc
b
c
{empty}
```
**The Magic Analogy:**
Imagine you are packing snacks for a trip. For each snack in your cabinet (e.g. apple, banana, cookie), you have a simple choice: "Do I put it in my bag, or do I leave it out?" With every choice, you branch into two separate paths!
- **Remember key:** For every index, make two recursive calls: one *including* the item, and one *excluding* it.

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
  // Move top n-1 disks to helper rod
  towerOfHanoi(n - 1, source, helper, destination);
  
  // Move bottom disk to destination rod
  print("Move disk $n from $source to $destination");
  
  // Move n-1 disks from helper to destination rod
  towerOfHanoi(n - 1, helper, destination, source);
}
```
**The Magic Analogy:**
You have a stack of plastic rings on Pole A. You want to move them all to Pole C, but you can never put a larger ring on top of a smaller ring. You use Pole B as a temporary holding spot. If you want to move 3 rings, you move the top 2 rings to the helper pole, move the biggest ring to the target pole, and then move the 2 rings from the helper pole back on top of the big ring!
- **Remember key:** Move `n-1` to helper, move bottom disk to destination, move `n-1` from helper to destination.

---

### 5.6 Sum of digits of a number (Recursive)
**Dart Code:**
```dart
void main() {
  print("Sum of digits of 254: ${sumOfDigits(254)}"); // Output: 11 (2 + 5 + 4)
}

int sumOfDigits(int n) {
  if (n == 0) return 0;
  return (n % 10) + sumOfDigits(n ~/ 10);
}
```
**The Magic Analogy:**
You are holding a bag of numbers: `254`. You take out the last digit `4`. To find the final sum, you add `4` to whatever the sum of the remaining numbers `25` is!
- **Remember key:** `Last Digit + sumOfDigits(remaining number)`.

---

## 6. Basic Data Structures (Toy Organizers)

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
  List<int> _storage = [];
  
  void push(int value) {
    _storage.add(value); // Put on top
  }
  
  int pop() {
    if (isEmpty) throw Exception("Stack is empty!");
    return _storage.removeLast(); // Take off top
  }
  
  int peek() {
    if (isEmpty) throw Exception("Stack is empty!");
    return _storage.last; // Just look at top
  }
  
  bool get isEmpty => _storage.isEmpty;
}
```
**The Magic Analogy:**
A stack is like a stack of pancakes. You always add new pancakes to the *top* (push) and you must eat the *top* pancake first (pop). You can't pull one from the middle without causing a mess! Last-In, First-Out (LIFO).
- **Remember key:** `add()` and `removeLast()` are how you implement a Stack in a List.

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
  List<int> _storage = [];
  
  void enqueue(int value) {
    _storage.add(value); // Stand at the back of the line
  }
  
  int dequeue() {
    if (isEmpty) throw Exception("Queue is empty!");
    return _storage.removeAt(0); // First person in line gets served
  }
  
  bool get isEmpty => _storage.isEmpty;
}
```
**The Magic Analogy:**
A queue is like a line of kids waiting for ice cream. The first kid to join the line gets served ice cream first (dequeue), and new kids must stand at the very back of the line (enqueue). First-In, First-Out (FIFO).
- **Remember key:** `add()` at the end, `removeAt(0)` from the front.

---

### 6.3 Check for balanced parentheses
**Dart Code:**
```dart
void main() {
  print("Is {()} balanced? ${isBalanced('{()}')}"); // Output: true
  print("Is {(} balanced? ${isBalanced('{(}')}");   // Output: false
}

bool isBalanced(String expression) {
  List<String> stack = [];
  Map<String, String> pairs = {')': '(', '}': '{', ']': '['};
  
  for (int i = 0; i < expression.length; i++) {
    String char = expression[i];
    
    if (char == '(' || char == '{' || char == '[') {
      stack.add(char); // Open bracket: Push to stack
    } else if (char == ')' || char == '}' || char == ']') {
      if (stack.isEmpty || stack.removeLast() != pairs[char]) {
        return false; // Mismatched or unmatched closed bracket!
      }
    }
  }
  return stack.isEmpty; // If stack is empty, everything matches!
}
```
**The Magic Analogy:**
Imagine you are wearing gloves and socks. Every time you put on a left glove/sock (opening bracket), you put its partner on a memory stack. When you take a right glove/sock off (closing bracket), it *must* match the item you put on the stack last.
- **Remember key:** Match open brackets using a stack. Pop and check on closed brackets.

---

### 6.4 Infix to Postfix conversion
**Dart Code:**
```dart
void main() {
  print("Postfix of A+B*C: ${infixToPostfix('A+B*C')}"); // Output: ABC*+
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
    
    // If it's a letter/number, add it directly to output
    if (RegExp(r'[a-zA-Z0-9]').hasMatch(char)) {
      result += char;
    } else if (char == '(') {
      stack.add(char);
    } else if (char == ')') {
      while (stack.isNotEmpty && stack.last != '(') {
        result += stack.removeLast();
      }
      stack.removeLast(); // Remove '('
    } else {
      // Operator found
      while (stack.isNotEmpty && getPrecedence(char) <= getPrecedence(stack.last)) {
        result += stack.removeLast();
      }
      stack.add(char);
    }
  }
  
  // Empty remaining stack
  while (stack.isNotEmpty) {
    result += stack.removeLast();
  }
  return result;
}
```
**The Magic Analogy:**
In normal math, we write operators between numbers (like `A + B`). A computer prefers reading numbers first, then operators (like `A B +`). We use a stack as a "waiting room" for operators. If a stronger operator (like `*`) arrives, it gets priority over a weaker one (like `+`).
- **Remember key:** Use a stack to hold operators and order them based on their math strength (precedence).

---

### 6.5 Evaluate Postfix expression
**Dart Code:**
```dart
void main() {
  print("Result of '231*+9-': ${evaluatePostfix('231*+9-')}"); // Output: -4
}

int evaluatePostfix(String exp) {
  List<int> stack = [];
  
  for (int i = 0; i < exp.length; i++) {
    String char = exp[i];
    
    if (RegExp(r'[0-9]').hasMatch(char)) {
      stack.add(int.parse(char));
    } else {
      // Operator found, pull two numbers to apply the math
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
**The Magic Analogy:**
You read instructions from left to right. When you read a number, you put it on the table. When you read an operator like `+`, you grab the last two numbers you put down, add them together, and place the result back on the table!
- **Remember key:** Push digits to a stack. Pop 2 operands when you hit an operator, math them, and push the result back.

---

## 7. Searching & Sorting (Finding & Organizing Toys)

---

### 7.1 Binary Search
**Dart Code:**
```dart
void main() {
  List<int> numbers = [2, 5, 8, 12, 16, 23, 38, 56, 72];
  print("Index of 23: ${binarySearch(numbers, 23)}"); // Output: 5
}

int binarySearch(List<int> sortedList, int target) {
  int low = 0;
  int high = sortedList.length - 1;
  
  while (low <= high) {
    int mid = low + ((high - low) ~/ 2);
    
    if (sortedList[mid] == target) {
      return mid; // Found it!
    } else if (sortedList[mid] < target) {
      low = mid + 1; // Search the right half
    } else {
      high = mid - 1; // Search the left half
    }
  }
  return -1; // Target is not in the list
}
```
**The Magic Analogy:**
Imagine you are looking for a word in a dictionary. Instead of flipping pages one-by-one from the start, you open the book exactly in the middle. If your word starts with a letter further down the alphabet, you tear away the left half of the book and repeat the search in the remaining right half!
- **Remember key:** List must be sorted. Halve the search space every single step.

---

### 7.2 Linear Search
**Dart Code:**
```dart
void main() {
  List<int> numbers = [5, 3, 8, 2, 9];
  print("Index of 2: ${linearSearch(numbers, 2)}"); // Output: 3
}

int linearSearch(List<int> list, int target) {
  for (int i = 0; i < list.length; i++) {
    if (list[i] == target) {
      return i; // Found it!
    }
  }
  return -1;
}
```
**The Magic Analogy:**
You lost your favorite lego brick. You look through your toy drawer by picking up every single brick one-by-one from the front to the back until you find the matching one!
- **Remember key:** Go element-by-element from index `0` to the end.

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
        // Swap adjacent elements
        int temp = list[j];
        list[j] = list[j + 1];
        list[j + 1] = temp;
      }
    }
  }
}
```
**The Magic Analogy:**
Imagine water bubbles rising in a glass. The largest bubbles are the heaviest/biggest numbers. You compare two side-by-side numbers, and swap them if the left one is larger than the right one. This pushes the largest number all the way to the end on each pass!
- **Remember key:** Compare neighbors and swap. The biggest number "bubbles" to the end on each round.

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
        minIndex = j; // Keep track of the absolute smallest
      }
    }
    // Swap the smallest item with the first unsorted item
    int temp = list[minIndex];
    list[minIndex] = list[i];
    list[i] = temp;
  }
}
```
**The Magic Analogy:**
You are sorting crayons. You scan the entire pile, find the absolute shortest crayon, and swap it with the crayon at the very start of the pile. Then you scan the rest, find the next shortest, and swap it to the second spot.
- **Remember key:** Search for the minimum value in the unsorted section and place it at the front.

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
    
    // Move elements that are greater than key to one position ahead
    while (j >= 0 && list[j] > key) {
      list[j + 1] = list[j];
      j--;
    }
    list[j + 1] = key; // Place the card in its correct spot
  }
}
```
**The Magic Analogy:**
Imagine you are playing cards. You hold a sorted hand. You draw a new card. You look at it, then slide it backwards past your larger cards until it fits in its perfect place!
- **Remember key:** Take the next item and insert it backward into its sorted position.

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
  
  // Add remaining items
  result.addAll(left.sublist(i));
  result.addAll(right.sublist(j));
  return result;
}
```
**The Magic Analogy:**
You have a messy pile of homework sheets. You split the pile into two. You give each pile to a friend to sort. Once they return the two sorted piles, you combine (merge) them by comparing the top sheet of each pile, taking the smaller one, and stacking it in the final pile!
- **Remember key:** Divide the list in half, sort recursively, and merge the sorted halves.

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
    
    // Sort items before and after pivot
    quickSort(list, low, pivotIndex - 1);
    quickSort(list, pivotIndex + 1, high);
  }
}

int partition(List<int> list, int low, int high) {
  int pivot = list[high]; // Choose the last element as the pivot
  int i = low - 1; // Index of smaller element
  
  for (int j = low; j < high; j++) {
    if (list[j] < pivot) {
      i++;
      // Swap list[i] and list[j]
      int temp = list[i];
      list[i] = list[j];
      list[j] = temp;
    }
  }
  // Place pivot in the correct position
  int temp = list[i + 1];
  list[i + 1] = list[high];
  list[high] = temp;
  
  return i + 1;
}
```
**The Magic Analogy:**
Pick a single kid in the class to be the "pivot" (say, the kid at the end of the line). Tell everyone who is shorter than the pivot to stand on the left side of the room, and everyone who is taller to stand on the right side. The pivot kid stands in the middle. Now do the same for the left group and the right group!
- **Remember key:** Partition items around a pivot so smaller go left, larger go right, then sort recursively.

---

## 8. Important Problem Solving (Riddles for Smart Brains)

---

### 8.1 Two Sum Problem
**Dart Code:**
```dart
void main() {
  List<int> numbers = [2, 7, 11, 15];
  print("Indices of sum: ${twoSum(numbers, 9)}"); // Output: [0, 1]
}

List<int> twoSum(List<int> list, int target) {
  Map<int, int> numMap = {}; // Map of value to its index
  
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
**The Magic Analogy:**
You are holding a target card `9`. You look at your first number block `2`. You need a `7` to reach your goal (`9 - 2 = 7`). You check your magic board: "Do I have a 7 yet?" No. So you write "I saw a 2 at index 0" on your board and move to the next number. When you see `7`, you check the board, see `2` is already there, and win!
- **Remember key:** `Complement = target - current`. Store seen values in a map.

---

### 8.2 Check if array contains a pair with given sum
**Dart Code:**
```dart
void main() {
  List<int> numbers = [1, 4, 45, 6, 10, 8];
  print("Has pair? ${hasPairWithSum(numbers, 16)}"); // Output: true (6 + 10)
}

bool hasPairWithSum(List<int> list, int sum) {
  Set<int> seen = {};
  for (int num in list) {
    if (seen.contains(sum - num)) {
      return true; // Found a pair!
    }
    seen.add(num);
  }
  return false;
}
```
**The Magic Analogy:**
Similar to Two Sum, but instead of remembering indices, you just need a yes/no! You walk along dropping numbers in a basket. Before you put a new number in, you check if the number you *need* to complete the sum is already in the basket.
- **Remember key:** Look for `sum - current` in the set of previously seen numbers.

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
  
  // Step 1: Find candidate
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
  
  // Step 2: Verify candidate
  int verificationCount = 0;
  for (int num in list) {
    if (num == candidate) verificationCount++;
  }
  
  return (verificationCount > list.length / 2) ? candidate : null;
}
```
**The Magic Analogy:**
Imagine an arena where different numbers are teams fighting. If two players of the same team meet, they team up (count grows). If they meet an enemy player from a different team, they knock each other out (count shrinks). The team that has more members than all other teams combined will have at least one player left standing in the end!
- **Remember key:** Boyer-Moore Voting algorithm cancels out different elements.

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
  leaders.add(maxFromRight); // Rightmost is always a leader
  
  // Walk backward
  for (int i = list.length - 2; i >= 0; i--) {
    if (list[i] > maxFromRight) {
      maxFromRight = list[i];
      leaders.add(maxFromRight);
    }
  }
  return leaders.reversed.toList(); // Reverse to keep original order
}
```
**The Magic Analogy:**
Imagine a row of people looking out at the ocean on their right. A person is a "leader" if they are taller than *everyone* standing to their right, so they have a clear view of the water. If you look from right to left, you only add a leader if they are taller than the tallest person you have seen so far!
- **Remember key:** Scan from right to left, keeping track of the maximum value.

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
**The Magic Analogy:**
Imagine building columns of blocks, leaving gaps between them. When it rains, water gets trapped in the valleys. The amount of water on top of any block depends on the shorter of the tallest walls to its left and right. We use two runners starting at both ends, moving inward to calculate this height!
- **Remember key:** Two pointers. Calculate water level at the current block based on the lower boundary height.

---

### 8.6 Maximum / Minimum in sliding window
**Dart Code:**
```dart
import 'dart:collection';

void main() {
  List<int> numbers = [1, 3, -1, -3, 5, 3, 6, 7];
  print("Max in sliding window of size 3: ${maxSlidingWindow(numbers, 3)}");
  // Output: [3, 3, 5, 5, 6, 7]
}

List<int> maxSlidingWindow(List<int> nums, int k) {
  List<int> result = [];
  if (nums.isEmpty || k <= 0) return result;
  
  // Double-ended queue storing indices
  DoubleLinkedQueue<int> deque = DoubleLinkedQueue<int>();
  
  for (int i = 0; i < nums.length; i++) {
    // 1. Remove indices outside the window bounds
    if (deque.isNotEmpty && deque.first == i - k) {
      deque.removeFirst();
    }
    
    // 2. Remove smaller elements from the back (they won't be needed)
    while (deque.isNotEmpty && nums[deque.last] < nums[i]) {
      deque.removeLast();
    }
    
    // 3. Add current index to deque
    deque.addLast(i);
    
    // 4. If window size is reached, add max to result
    if (i >= k - 1) {
      result.add(nums[deque.first]);
    }
  }
  return result;
}
```
**The Magic Analogy:**
You are holding a cardboard frame of size 3. You slide it over a row of numbers. To quickly know the largest number inside the frame, you keep a "Leaderboard" (deque). Every time you slide the frame to include a new number, you kick off the leaderboard any older, smaller numbers that have no chance of winning anymore. The number at the front of your leaderboard is always the winner for that frame!
- **Remember key:** Use a Deque of indices to keep elements sorted inside the active sliding window.
