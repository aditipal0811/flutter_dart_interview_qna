# Dart Interview Questions - Basic To Advanced

This file contains Dart-only interview questions with easy English answers and practical code examples. Use it to prepare for Dart fundamentals, OOP, null safety, async programming, collections, advanced language features, and common coding patterns.

Interview tip: first give a short answer, then explain one example. If the interviewer asks deeper, then discuss edge cases and performance.

---

## Dart Basics

### 1. What is Dart?
Dart is a programming language created by Google. It is used to build Flutter apps, server-side apps, command-line tools, and web apps.

**Interview answer:** Dart is strongly typed, object-oriented, null-safe, and supports both synchronous and asynchronous programming.

### 2. Why is Dart used with Flutter?
Dart is used with Flutter because it compiles fast during development, supports hot reload, and can compile to native machine code for mobile apps.

### 3. Is Dart statically typed or dynamically typed?
Dart is statically typed, which means variable types are checked at compile time.

But Dart also supports `dynamic` when we intentionally want runtime typing.

### 4. What is the difference between var, final, and const?
`var` lets Dart infer the type.

`final` can be assigned only once at runtime.

`const` is a compile-time constant.

```dart
var name = 'Amit'; // Dart infers String
final currentTime = DateTime.now(); // runtime value
const pi = 3.14; // compile-time value
```

### 5. Difference between final and const.
`final` value is fixed after assignment, but it can be assigned at runtime.

`const` value must be known at compile time.

```dart
final today = DateTime.now(); // valid
// const today = DateTime.now(); // invalid
```

### 6. What is dynamic in Dart?
`dynamic` means Dart will not check the type at compile time. We can assign any value and call any method, but it can fail at runtime.

```dart
dynamic value = 'hello';
value = 10;
```

**Interview answer:** I avoid `dynamic` unless I am dealing with unknown JSON or third-party data.

### 7. Difference between dynamic, Object, and var.
`var` infers the type and keeps it fixed.

`Object` can store any non-null value, but we must cast/check before calling specific methods.

`dynamic` skips compile-time checks.

```dart
var a = 'text'; // String
Object b = 'text';
dynamic c = 'text';

print((b as String).length);
print(c.length);
```

### 8. What are comments in Dart?
Dart supports single-line, multi-line, and documentation comments.

```dart
// Single-line comment

/*
 Multi-line comment
*/

/// Documentation comment
void login() {}
```

### 9. What is type inference?
Type inference means Dart automatically understands the variable type from the assigned value.

```dart
var count = 10; // int
var name = 'Rahul'; // String
```

### 10. What is cascade notation?
Cascade notation `..` lets us perform multiple operations on the same object.

```dart
final buffer = StringBuffer()
  ..write('Hello')
  ..write(' ')
  ..write('Dart');

print(buffer.toString());
```

---

## Data Types

### 11. What are common Dart data types?
Common Dart data types are:

- `int`
- `double`
- `num`
- `String`
- `bool`
- `List`
- `Set`
- `Map`
- `Object`
- `dynamic`

### 12. Difference between int, double, and num.
`int` stores whole numbers.

`double` stores decimal numbers.

`num` can store both `int` and `double`.

```dart
int age = 25;
double price = 99.5;
num value = 10;
value = 10.5;
```

### 13. What is String interpolation?
String interpolation means inserting variable values inside a string using `$`.

```dart
String name = 'Amit';
int age = 25;

print('Name is $name and age is $age');
print('Next age is ${age + 1}');
```

### 14. What is bool in Dart?
`bool` stores `true` or `false`.

```dart
bool isLoggedIn = true;
```

### 15. What is rune in Dart?
Runes represent Unicode code points of a string.

```dart
String value = 'A';
print(value.runes); // Unicode values
```

### 16. What is Symbol in Dart?
`Symbol` represents an operator or identifier name. It is rarely used in normal Flutter app development.

```dart
Symbol symbol = #myVariable;
```

---

## Operators

### 17. What are arithmetic operators in Dart?
Arithmetic operators are `+`, `-`, `*`, `/`, `~/`, and `%`.

```dart
print(10 / 3); // 3.3333
print(10 ~/ 3); // 3
print(10 % 3); // 1
```

### 18. What is the null-aware operator?
Null-aware operators help handle nullable values safely.

```dart
String? name;

print(name ?? 'Guest');
name ??= 'Default';
print(name?.length);
```

### 19. Difference between ?? and ??=.
`??` returns the right-side value if the left-side value is null.

`??=` assigns a value only if the variable is null.

```dart
String? name;
print(name ?? 'Guest');

name ??= 'Amit';
```

### 20. What is ?. operator?
`?.` safely accesses a property or method only if the object is not null.

```dart
String? name;
print(name?.length); // null, no crash
```

### 21. What is ! operator in null safety?
`!` tells Dart that a nullable value is definitely not null.

```dart
String? name = 'Amit';
print(name!.length);
```

Use it carefully. If value is null, it throws a runtime error.

---

## Control Flow

### 22. What are if-else statements?
`if-else` is used for decision making.

```dart
int age = 20;

if (age >= 18) {
  print('Adult');
} else {
  print('Minor');
}
```

### 23. What is switch in Dart?
`switch` is used when one value can match multiple cases.

```dart
String role = 'admin';

switch (role) {
  case 'admin':
    print('Admin user');
    break;
  case 'user':
    print('Normal user');
    break;
  default:
    print('Unknown role');
}
```

### 24. What are loops in Dart?
Dart supports `for`, `for-in`, `while`, and `do-while`.

```dart
for (int i = 0; i < 3; i++) {
  print(i);
}

for (final item in ['a', 'b', 'c']) {
  print(item);
}
```

### 25. Difference between break and continue.
`break` stops the loop.

`continue` skips the current iteration and moves to the next one.

```dart
for (int i = 1; i <= 5; i++) {
  if (i == 3) continue;
  if (i == 5) break;
  print(i);
}
```

---

## Functions

### 26. How do you write a function in Dart?
```dart
int add(int a, int b) {
  return a + b;
}
```

### 27. What is arrow function syntax?
Arrow syntax is used for short single-expression functions.

```dart
int add(int a, int b) => a + b;
```

### 28. What are optional positional parameters?
Optional positional parameters are written inside `[]`.

```dart
void greet(String name, [String? message]) {
  print('Hello $name ${message ?? ''}');
}

greet('Amit');
greet('Amit', 'Good morning');
```

### 29. What are named parameters?
Named parameters are written inside `{}` and called by name.

```dart
void createUser({required String name, int age = 18}) {
  print('$name $age');
}

createUser(name: 'Amit', age: 25);
```

### 30. What is required keyword?
`required` means the named parameter must be passed.

```dart
void login({required String email, required String password}) {}
```

### 31. What is a higher-order function?
A higher-order function accepts another function as a parameter or returns a function.

```dart
void performOperation(int a, int b, int Function(int, int) operation) {
  print(operation(a, b));
}

performOperation(10, 5, (a, b) => a + b);
```

### 32. What is anonymous function?
An anonymous function has no name. It is commonly used in callbacks.

```dart
final numbers = [1, 2, 3];
numbers.forEach((number) {
  print(number);
});
```

### 33. What is lexical scope?
Lexical scope means a function can access variables declared in its outer scope.

```dart
void outer() {
  String message = 'Hello';

  void inner() {
    print(message);
  }

  inner();
}
```

### 34. What is closure in Dart?
A closure is a function that remembers variables from its outer scope even after the outer function has finished.

```dart
Function counter() {
  int count = 0;

  return () {
    count++;
    return count;
  };
}

final next = counter();
print(next()); // 1
print(next()); // 2
```

---

## Collections

### 35. What is List in Dart?
`List` is an ordered collection that allows duplicate values.

```dart
List<int> numbers = [1, 2, 3, 3];
numbers.add(4);
```

### 36. What is Set in Dart?
`Set` is an unordered collection of unique values.

```dart
Set<int> uniqueNumbers = {1, 2, 3, 3};
print(uniqueNumbers); // {1, 2, 3}
```

### 37. What is Map in Dart?
`Map` stores key-value pairs.

```dart
Map<String, int> marks = {
  'math': 90,
  'science': 85,
};

print(marks['math']);
```

### 38. Difference between List, Set, and Map.
`List` stores ordered values and allows duplicates.

`Set` stores unique values.

`Map` stores key-value pairs.

### 39. What are collection if and collection for?
They allow conditions and loops inside collection literals.

```dart
bool isAdmin = true;

final menu = [
  'Home',
  if (isAdmin) 'Admin',
];

final numbers = [
  for (int i = 1; i <= 3; i++) i,
];
```

### 40. What is spread operator?
Spread operator `...` adds all items of one collection into another.

```dart
final a = [1, 2];
final b = [3, 4];
final c = [...a, ...b];
```

### 41. What is null-aware spread operator?
`...?` spreads a collection only if it is not null.

```dart
List<int>? values;
final result = [0, ...?values];
```

### 42. Difference between map, where, and reduce.
`map` transforms each item.

`where` filters items.

`reduce` combines items into one value.

```dart
final numbers = [1, 2, 3, 4];

print(numbers.map((e) => e * 2).toList());
print(numbers.where((e) => e.isEven).toList());
print(numbers.reduce((a, b) => a + b));
```

### 43. Difference between reduce and fold.
`reduce` uses the first item as initial value and fails on empty list.

`fold` accepts an initial value and works with empty list.

```dart
final numbers = <int>[];

final sum = numbers.fold<int>(0, (total, value) => total + value);
```

### 44. How do you sort a list in Dart?
```dart
final numbers = [4, 1, 3, 2];
numbers.sort();

final users = ['Rahul', 'Amit', 'Zoya'];
users.sort((a, b) => a.compareTo(b));
```

### 45. How do you remove duplicates from a list?
Convert it to a set and back to a list.

```dart
final numbers = [1, 2, 2, 3];
final unique = numbers.toSet().toList();
```

---

## Null Safety

### 46. What is null safety?
Null safety prevents variables from becoming null unless we explicitly allow it.

```dart
String name = 'Amit';
String? optionalName;
```

### 47. What is nullable type?
A nullable type can store null. It is written using `?`.

```dart
int? age;
```

### 48. What is non-nullable type?
A non-nullable type cannot store null.

```dart
int age = 20;
```

### 49. What is late variable?
`late` means the value will be assigned later before use.

```dart
late String token;

void setToken() {
  token = 'abc';
}
```

### 50. What is late final?
`late final` means the value is initialized later but only once.

```dart
late final String userId;

void init() {
  userId = '101';
}
```

### 51. What is null assertion operator?
The null assertion operator `!` converts a nullable value to non-nullable.

```dart
String? name = 'Amit';
print(name!.length);
```

Use it only when you are sure the value is not null.

---

## Object-Oriented Programming

### 52. What is a class in Dart?
A class is a blueprint for creating objects.

```dart
class User {
  String name;

  User(this.name);
}
```

### 53. What is an object?
An object is an instance of a class.

```dart
final user = User('Amit');
```

### 54. What is constructor?
A constructor is used to create and initialize an object.

```dart
class User {
  final String name;

  User(this.name);
}
```

### 55. What is named constructor?
Named constructor provides another way to create an object.

```dart
class User {
  final String name;

  User(this.name);

  User.guest() : name = 'Guest';
}

final guest = User.guest();
```

### 56. What is factory constructor?
A factory constructor controls object creation and can return an existing object.

```dart
class Logger {
  static final Logger _instance = Logger._internal();

  factory Logger() {
    return _instance;
  }

  Logger._internal();
}
```

### 57. How do you write a singleton class in Dart?
Singleton means only one instance of a class exists.

```dart
class AppConfig {
  AppConfig._privateConstructor();

  static final AppConfig instance = AppConfig._privateConstructor();

  String baseUrl = 'https://api.example.com';
}

void main() {
  final config = AppConfig.instance;
  print(config.baseUrl);
}
```

**Interview answer:** I use singleton for config, logger, database helper, or shared service, but I avoid overusing it for business logic.

### 58. What is private variable in Dart?
A variable or method starting with `_` is private to its library/file.

```dart
class Account {
  double _balance = 0;

  double get balance => _balance;
}
```

### 59. What are getters and setters?
Getters read a value. Setters update a value with control.

```dart
class User {
  String _name = '';

  String get name => _name;

  set name(String value) {
    if (value.isNotEmpty) {
      _name = value;
    }
  }
}
```

### 60. What is inheritance?
Inheritance allows one class to reuse another class using `extends`.

```dart
class Animal {
  void eat() => print('Eating');
}

class Dog extends Animal {
  void bark() => print('Barking');
}
```

### 61. What is method overriding?
Method overriding means child class changes parent class method behavior.

```dart
class Animal {
  void sound() => print('Some sound');
}

class Dog extends Animal {
  @override
  void sound() => print('Bark');
}
```

### 62. Does Dart support method overloading?
Dart does not support traditional method overloading. We use optional or named parameters.

```dart
void printUser({String? name, int? age}) {
  print('$name $age');
}
```

### 63. What is abstract class?
An abstract class cannot be directly instantiated. It can define methods that child classes must implement.

```dart
abstract class PaymentService {
  Future<void> pay(double amount);
}

class UpiPayment implements PaymentService {
  @override
  Future<void> pay(double amount) async {
    print('Paid by UPI: $amount');
  }
}
```

### 64. What is interface in Dart?
In Dart, every class can be used as an interface using `implements`.

```dart
class AuthRepository {
  Future<void> login() async {}
}

class FakeAuthRepository implements AuthRepository {
  @override
  Future<void> login() async {}
}
```

### 65. Difference between extends and implements.
`extends` reuses parent class behavior.

`implements` follows a contract and requires implementing all methods.

### 66. What is mixin in Dart?
A mixin lets a class reuse methods without normal inheritance.

```dart
mixin LoggerMixin {
  void log(String message) {
    print('LOG: $message');
  }
}

class ApiService with LoggerMixin {
  void fetchData() {
    log('Fetching data');
  }
}
```

### 67. How do you restrict a mixin to specific classes?
Use `on` keyword.

```dart
class BaseService {
  void connect() => print('Connected');
}

mixin RetryMixin on BaseService {
  void retry() {
    connect();
    print('Retrying');
  }
}

class ApiService extends BaseService with RetryMixin {}
```

### 68. Abstract class vs mixin.
Abstract class is used for base contract or shared base behavior.

Mixin is used to add reusable ability to multiple classes.

**Memory line:** Abstract class defines identity. Mixin adds capability.

### 69. What is polymorphism?
Polymorphism means different classes can be used through the same parent type or interface.

```dart
abstract class Shape {
  double area();
}

class Circle implements Shape {
  @override
  double area() => 78.5;
}

class Square implements Shape {
  @override
  double area() => 100;
}

void printArea(Shape shape) {
  print(shape.area());
}
```

### 70. What is encapsulation?
Encapsulation means hiding internal data and exposing controlled methods.

```dart
class Counter {
  int _count = 0;

  int get count => _count;

  void increment() {
    _count++;
  }
}
```

### 71. What is composition?
Composition means one class uses another class instead of extending it.

```dart
class Engine {
  void start() => print('Engine started');
}

class Car {
  final Engine engine;

  Car(this.engine);

  void start() => engine.start();
}
```

**Interview answer:** Composition is often more flexible than inheritance.

---

## Extension Methods

### 72. What are extension methods?
Extension methods add new methods or getters to existing types without changing their source code.

```dart
extension StringValidation on String {
  bool get isEmail => contains('@') && contains('.');
}

void main() {
  print('test@gmail.com'.isEmail);
}
```

### 73. Where do you use extensions in projects?
I use extensions for:

- Date formatting
- String validation
- Number formatting
- Widget spacing helpers
- Mapping enum values to display text

### 74. Can extensions have fields?
Extensions cannot store instance fields. They can only add methods, getters, setters, and operators.

---

## Enums

### 75. What is enum in Dart?
Enum represents a fixed set of constant values.

```dart
enum UserRole {
  admin,
  manager,
  user,
}
```

### 76. What are enhanced enums?
Enhanced enums can have fields, constructors, getters, and methods.

```dart
enum OrderStatus {
  pending('Pending'),
  shipped('Shipped'),
  delivered('Delivered');

  final String label;
  const OrderStatus(this.label);
}
```

### 77. How do you use enum in switch?
```dart
void printRole(UserRole role) {
  switch (role) {
    case UserRole.admin:
      print('Admin');
      break;
    case UserRole.manager:
      print('Manager');
      break;
    case UserRole.user:
      print('User');
      break;
  }
}
```

---

## Generics

### 78. What are generics in Dart?
Generics allow code to work with different types safely.

```dart
class Box<T> {
  final T value;

  Box(this.value);
}

final intBox = Box<int>(10);
final stringBox = Box<String>('Hello');
```

### 79. Why do we use generics?
Generics improve type safety and code reuse.

Example: `List<String>` ensures the list only contains strings.

### 80. Write a generic API response class.
```dart
class ApiResponse<T> {
  final bool success;
  final T? data;
  final String? error;

  ApiResponse.success(this.data)
      : success = true,
        error = null;

  ApiResponse.error(this.error)
      : success = false,
        data = null;
}
```

### 81. What is generic constraint?
Generic constraint limits the allowed type.

```dart
class Repository<T extends Object> {
  final List<T> items = [];
}
```

---

## Async Programming

### 82. What is Future in Dart?
`Future` represents a value that will be available later.

```dart
Future<String> fetchName() async {
  return 'Amit';
}
```

### 83. What is async and await?
`async` marks a function as asynchronous. `await` waits for a future result without blocking the event loop.

```dart
Future<void> loadUser() async {
  final name = await fetchName();
  print(name);
}
```

### 84. What is Stream in Dart?
`Stream` represents multiple async values over time.

```dart
Stream<int> countStream() async* {
  for (int i = 1; i <= 3; i++) {
    await Future.delayed(const Duration(seconds: 1));
    yield i;
  }
}
```

### 85. Difference between Future and Stream.
Future returns one async value.

Stream returns many async values over time.

### 86. What is async* and yield?
`async*` creates a stream. `yield` emits values from the stream.

```dart
Stream<String> names() async* {
  yield 'Amit';
  yield 'Rahul';
}
```

### 87. What is await for?
`await for` listens to stream values one by one.

```dart
Future<void> listenNumbers() async {
  await for (final value in countStream()) {
    print(value);
  }
}
```

### 88. What is StreamController?
`StreamController` lets us manually add data, errors, and close a stream.

```dart
final controller = StreamController<int>();

void main() {
  controller.stream.listen(print);
  controller.add(1);
  controller.add(2);
  controller.close();
}
```

Add import:

```dart
import 'dart:async';
```

### 89. Single subscription stream vs broadcast stream.
Single subscription stream can have only one listener.

Broadcast stream can have multiple listeners.

```dart
final controller = StreamController<int>.broadcast();
```

### 90. How do you handle Future errors?
Use `try-catch` with `await`.

```dart
try {
  final data = await fetchName();
  print(data);
} catch (e) {
  print('Error: $e');
}
```

### 91. What is Future.wait?
`Future.wait` runs multiple futures together and waits for all results.

```dart
final results = await Future.wait([
  fetchName(),
  fetchName(),
]);
```

### 92. What is completer?
`Completer` lets us manually complete a Future.

```dart
final completer = Completer<String>();

Future<String> get result => completer.future;

void completeWork() {
  completer.complete('Done');
}
```

---

## Isolates

### 93. What is isolate in Dart?
An isolate is a separate execution worker with its own memory.

**Interview answer:** Isolates are used for CPU-heavy tasks so the main isolate does not freeze.

### 94. Is isolate same as thread?
No. Threads can share memory, but Dart isolates do not share memory. Isolates communicate using messages.

### 95. How do isolates communicate?
They communicate using `SendPort` and `ReceivePort`.

```dart
import 'dart:isolate';

void worker(SendPort sendPort) {
  sendPort.send('Work completed');
}

Future<void> main() async {
  final receivePort = ReceivePort();
  await Isolate.spawn(worker, receivePort.sendPort);

  receivePort.listen((message) {
    print(message);
    receivePort.close();
  });
}
```

### 96. What tasks should be moved to isolate?
Move CPU-heavy tasks like:

- Large JSON parsing
- Image processing
- Encryption
- File compression
- Heavy calculations

### 97. Difference between async/await and isolate.
`async/await` handles waiting without blocking, but work still runs on same isolate.

Isolate runs heavy work on a separate worker.

---

## Exception Handling

### 98. How do you handle exceptions in Dart?
Use `try-catch-finally`.

```dart
try {
  final result = 10 ~/ 0;
  print(result);
} catch (e) {
  print('Error: $e');
} finally {
  print('Always runs');
}
```

### 99. Difference between throw and rethrow.
`throw` throws a new or existing error.

`rethrow` throws the same caught error while preserving stack trace.

```dart
try {
  throw Exception('Failed');
} catch (e) {
  rethrow;
}
```

### 100. How do you create custom exception?
```dart
class ApiException implements Exception {
  final String message;

  ApiException(this.message);

  @override
  String toString() => 'ApiException: $message';
}
```

### 101. What is finally block?
`finally` runs whether error happens or not. It is used for cleanup.

---

## JSON And Model Classes

### 102. How do you parse JSON in Dart?
Use `dart:convert`.

```dart
import 'dart:convert';

void main() {
  final jsonString = '{"id":1,"name":"Amit"}';
  final map = jsonDecode(jsonString) as Map<String, dynamic>;
  print(map['name']);
}
```

### 103. How do you create model from JSON?
```dart
class User {
  final int id;
  final String name;

  User({
    required this.id,
    required this.name,
  });

  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'] as int,
      name: json['name'] as String,
    );
  }

  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
    };
  }
}
```

### 104. Why do we use fromJson and toJson?
`fromJson` converts API/map data into Dart object.

`toJson` converts Dart object into map/json for API or storage.

---

## Equality, HashCode, And Immutability

### 105. Why override == and hashCode?
By default, objects are compared by reference. Override `==` and `hashCode` to compare by values.

```dart
class User {
  final int id;
  final String name;

  User(this.id, this.name);

  @override
  bool operator ==(Object other) {
    return other is User && other.id == id && other.name == name;
  }

  @override
  int get hashCode => Object.hash(id, name);
}
```

### 106. What is immutable class?
An immutable class cannot be changed after object creation.

```dart
class User {
  final int id;
  final String name;

  const User({
    required this.id,
    required this.name,
  });
}
```

### 107. What is copyWith?
`copyWith` creates a new object by changing only selected fields.

```dart
class User {
  final String name;
  final int age;

  const User({required this.name, required this.age});

  User copyWith({String? name, int? age}) {
    return User(
      name: name ?? this.name,
      age: age ?? this.age,
    );
  }
}
```

---

## Dart 3 Class Modifiers

### 108. What are class modifiers in Dart?
Class modifiers control how classes can be used, extended, or implemented.

Common modifiers:

- `abstract`
- `base`
- `interface`
- `final`
- `sealed`
- `mixin`

### 109. What is sealed class?
A `sealed` class restricts subclassing to the same library. It is useful for fixed state types.

```dart
sealed class ApiState {}

class Loading extends ApiState {}

class Success extends ApiState {
  final String data;
  Success(this.data);
}

class Error extends ApiState {
  final String message;
  Error(this.message);
}
```

### 110. What is final class?
A `final` class cannot be extended or implemented outside its library.

```dart
final class AppConstants {}
```

### 111. What is interface class?
An `interface class` can be implemented outside the library but cannot be extended outside the library.

```dart
interface class AuthService {
  void login() {}
}

class ApiAuthService implements AuthService {
  @override
  void login() {}
}
```

### 112. What is base class?
A `base` class allows inheritance but forces child classes to also be `base`, `final`, or `sealed`.

```dart
base class Animal {}

base class Dog extends Animal {}
```

---

## Pattern Matching And Records

### 113. What are records in Dart?
Records allow grouping multiple values without creating a class.

```dart
(String, int) user = ('Amit', 25);
print(user.$1);
print(user.$2);
```

Named record fields:

```dart
({String name, int age}) user = (name: 'Amit', age: 25);
print(user.name);
```

### 114. What is pattern matching?
Pattern matching lets us destructure and check values in a clean way.

```dart
final user = ('Amit', 25);
final (name, age) = user;

print(name);
print(age);
```

### 115. What is switch expression?
Switch expression returns a value.

```dart
String roleLabel(String role) {
  return switch (role) {
    'admin' => 'Admin User',
    'manager' => 'Manager',
    _ => 'Normal User',
  };
}
```

---

## Memory And Performance

### 116. How does Dart garbage collection work?
Dart automatically frees memory for objects that are no longer used.

**Interview answer:** We still need to close streams, timers, and controllers because they can keep references alive.

### 117. How can memory leaks happen in Dart/Flutter?
Memory leaks can happen when streams, timers, controllers, or listeners are not cancelled/disposed.

### 118. How do you improve Dart code performance?
Use proper data structures, avoid unnecessary object creation, avoid heavy work on main isolate, use typed collections, and move CPU-heavy work to isolates.

### 119. List vs Set lookup performance.
List search is usually `O(n)`.

Set lookup is average `O(1)`.

Use Set when you frequently check whether a value exists.

---

## SOLID And Design Patterns In Dart

### 120. How do you apply Single Responsibility Principle in Dart?
Keep one class focused on one job.

Example:

- `ApiService` calls API.
- `UserRepository` manages user data.
- `UserController` manages state.

### 121. How do you apply Dependency Inversion in Dart?
Depend on abstract contracts instead of concrete classes.

```dart
abstract class AuthRepository {
  Future<void> login();
}

class LoginUseCase {
  final AuthRepository repository;

  LoginUseCase(this.repository);

  Future<void> call() => repository.login();
}
```

### 122. Write Factory pattern example in Dart.
```dart
abstract class Payment {
  void pay();
}

class CardPayment implements Payment {
  @override
  void pay() => print('Card payment');
}

class UpiPayment implements Payment {
  @override
  void pay() => print('UPI payment');
}

class PaymentFactory {
  static Payment create(String type) {
    switch (type) {
      case 'card':
        return CardPayment();
      case 'upi':
        return UpiPayment();
      default:
        throw Exception('Invalid payment type');
    }
  }
}
```

### 123. Write Repository pattern example in Dart.
```dart
abstract class UserRepository {
  Future<String> getUserName();
}

class ApiUserRepository implements UserRepository {
  @override
  Future<String> getUserName() async {
    return 'Amit';
  }
}

class GetUserNameUseCase {
  final UserRepository repository;

  GetUserNameUseCase(this.repository);

  Future<String> call() => repository.getUserName();
}
```

### 124. Write Strategy pattern example in Dart.
```dart
abstract class DiscountStrategy {
  double apply(double price);
}

class NoDiscount implements DiscountStrategy {
  @override
  double apply(double price) => price;
}

class TenPercentDiscount implements DiscountStrategy {
  @override
  double apply(double price) => price * 0.9;
}

class Cart {
  final DiscountStrategy discountStrategy;

  Cart(this.discountStrategy);

  double checkout(double price) {
    return discountStrategy.apply(price);
  }
}
```

---

## Practical Interview Coding Snippets

### 125. How do you write debounce in Dart?
```dart
import 'dart:async';

class Debouncer {
  final Duration delay;
  Timer? _timer;

  Debouncer(this.delay);

  void run(void Function() action) {
    _timer?.cancel();
    _timer = Timer(delay, action);
  }

  void dispose() {
    _timer?.cancel();
  }
}
```

### 126. How do you write throttle in Dart?
```dart
class Throttler {
  final Duration delay;
  bool _ready = true;

  Throttler(this.delay);

  void run(void Function() action) {
    if (!_ready) return;

    _ready = false;
    action();

    Timer(delay, () {
      _ready = true;
    });
  }
}
```

Add import:

```dart
import 'dart:async';
```

### 127. How do you safely cast JSON values?
```dart
String readName(Map<String, dynamic> json) {
  final value = json['name'];
  if (value is String) {
    return value;
  }
  return 'Unknown';
}
```

### 128. How do you make a class callable like a function?
Use `call()` method.

```dart
class Add {
  int call(int a, int b) => a + b;
}

void main() {
  final add = Add();
  print(add(2, 3));
}
```

### 129. How do you use typedef?
`typedef` gives a function type a readable name.

```dart
typedef Validator = bool Function(String value);

bool isNotEmpty(String value) => value.isNotEmpty;

void validate(String value, Validator validator) {
  print(validator(value));
}
```

### 130. How do you create an extension for date formatting?
```dart
extension DateFormatExtension on DateTime {
  String get ddMMyyyy {
    return '$day/$month/$year';
  }
}

void main() {
  print(DateTime.now().ddMMyyyy);
}
```

---

## Quick Revision Lines

- Dart is strongly typed and null-safe.
- `final` is runtime constant, `const` is compile-time constant.
- `dynamic` skips type checking, so use it carefully.
- `Future` gives one async value, `Stream` gives many values.
- `async/await` does not create a new thread.
- Isolates are used for CPU-heavy work.
- Mixin adds reusable behavior.
- Abstract class defines contract or base behavior.
- Factory constructor controls object creation.
- Singleton creates only one shared instance.
- Extension methods add helper behavior to existing types.
- Generics improve type safety and reusability.
- `Set` is good for fast existence checks.
- `fold` is safer than `reduce` for empty lists.
- Always close streams, timers, and controllers when not needed.
