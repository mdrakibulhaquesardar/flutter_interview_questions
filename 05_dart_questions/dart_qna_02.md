# Dart Language - প্রশ্নোত্তর সেট ২

## Collections এবং Generics

### প্রশ্ন ৬: Dart-এ Collections (List, Set, Map) কীভাবে ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- **Collections** হলো multiple values store করার জন্য ব্যবহৃত data structures। Dart-এ তিন ধরনের collection আছে: List, Set, এবং Map।

**Collection Types:**
- **List**: Ordered collection of elements
- **Set**: Unordered collection of unique elements
- **Map**: Key-value pairs collection

**উদাহরণ:**

```dart
void collectionsDemo() {
  // List examples
  List<String> names = ['John', 'Jane', 'Bob'];
  names.add('Alice');
  names.remove('Bob');
  
  // List operations
  final first = names.first;
  final last = names.last;
  final length = names.length;
  
  // Set examples
  Set<int> numbers = {1, 2, 3, 4, 5};
  numbers.add(6);
  numbers.remove(1);
  
  // Set operations
  final contains = numbers.contains(3);
  final union = numbers.union({5, 6, 7});
  
  // Map examples
  Map<String, dynamic> person = {
    'name': 'John',
    'age': 25,
    'city': 'Dhaka',
  };
  
  person['email'] = 'john@example.com';
  person.remove('city');
  
  // Map operations
  final keys = person.keys;
  final values = person.values;
  final hasKey = person.containsKey('name');
}

// Generic collections
class CollectionExample<T> {
  final List<T> items;
  
  CollectionExample(this.items);
  
  void addItem(T item) => items.add(item);
  void removeItem(T item) => items.remove(item);
  bool contains(T item) => items.contains(item);
}
```

**Interview Tips:**
- List ordered, Set unordered
- Map key-value pairs store করে
- Generics type safety provide করে
- Collections mutable by default

---

### প্রশ্ন ৭: Generics কী এবং এটা কীভাবে implement করবেন?

**উত্তর (ডিটেইল):**

- **Generics** হলো type-safe code লিখার জন্য ব্যবহৃত mechanism। এটা different types-এর জন্য reusable code লিখতে সাহায্য করে।

**Generic Features:**
- Type parameters
- Generic classes
- Generic methods
- Generic collections

**উদাহরণ:**

```dart
// Generic class
class Box<T> {
  final T value;
  
  Box(this.value);
  
  T getValue() => value;
  
  bool isEmpty() => value == null;
}

// Generic method
T findFirst<T>(List<T> list, bool Function(T) predicate) {
  for (final item in list) {
    if (predicate(item)) return item;
  }
  throw Exception('Not found');
}

// Usage
void genericDemo() {
  final stringBox = Box<String>('Hello');
  final intBox = Box<int>(42);
  
  final numbers = [1, 2, 3, 4, 5];
  final firstEven = findFirst(numbers, (n) => n % 2 == 0);
}
```

**Interview Tips:**
- Generics compile-time type safety provide করে
- Type parameters `<T>` দিয়ে define হয়
- Generic collections type-safe data store করে
- Generic methods reusable code লিখতে সাহায্য করে

---

### প্রশ্ন ৮: Async Programming (Future, Stream) কীভাবে handle করবেন?

**উত্তর (ডিটেইল):**

- **Async Programming** হলো non-blocking operations handle করার জন্য ব্যবহৃত technique। Dart-এ Future এবং Stream ব্যবহার করে async programming করা হয়।

**Async Concepts:**
- Future: Single async operation
- Stream: Multiple async operations
- async/await: Modern async syntax

**উদাহরণ:**

```dart
// Future examples
Future<String> fetchUserData() async {
  await Future.delayed(Duration(seconds: 2));
  return 'User data loaded';
}

// Stream examples
Stream<int> countStream() async* {
  for (int i = 1; i <= 5; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}

// Usage
void asyncDemo() async {
  final userData = await fetchUserData();
  print(userData);
  
  await for (final count in countStream()) {
    print('Count: $count');
  }
}
```

**Interview Tips:**
- Future single value return করে
- Stream multiple values yield করে
- async/await code readable করে
- Error handling async operations-এ গুরুত্বপূর্ণ

---

### প্রশ্ন ৯: Error Handling কীভাবে implement করবেন?

**উত্তর (ডিটেইল):**

- **Error Handling** হলো runtime errors handle করার জন্য ব্যবহৃত mechanism। Dart-এ try-catch, throw, এবং rethrow ব্যবহার করে error handling করা হয়।

**Error Handling Methods:**
- try-catch blocks
- throw statements
- rethrow keyword
- Custom exceptions

**উদাহরণ:**

```dart
// Custom exception
class CustomException implements Exception {
  final String message;
  CustomException(this.message);
  
  @override
  String toString() => 'CustomException: $message';
}

// Error handling
void errorHandlingDemo() {
  try {
    // Risky operation
    final result = 10 ~/ 0; // Division by zero
  } on IntegerDivisionByZeroException {
    print('Cannot divide by zero');
  } on FormatException catch (e) {
    print('Format error: $e');
  } catch (e, stackTrace) {
    print('Unexpected error: $e');
    print('Stack trace: $stackTrace');
  } finally {
    print('Cleanup code');
  }
}

// Function that throws
void riskyFunction() {
  throw CustomException('Something went wrong');
}
```

**Interview Tips:**
- try-catch runtime errors handle করে
- on keyword specific exceptions catch করে
- finally block cleanup code-এর জন্য
- Custom exceptions meaningful error messages provide করে

---

### প্রশ্ন ১০: Dart-এ Testing কীভাবে implement করবেন?

**উত্তর (ডিটেইল):**

- **Testing** হলো code-এর correctness verify করার জন্য ব্যবহৃত process। Dart-এ built-in testing framework আছে।

**Testing Types:**
- Unit tests
- Widget tests
- Integration tests
- Golden tests

**উদাহরণ:**

```dart
// Simple function to test
int add(int a, int b) => a + b;

// Test file
import 'package:test/test.dart';

void main() {
  group('Math operations', () {
    test('add function adds two numbers', () {
      expect(add(2, 3), equals(5));
      expect(add(-1, 1), equals(0));
      expect(add(0, 0), equals(0));
    });
    
    test('add function handles large numbers', () {
      expect(add(1000000, 2000000), equals(3000000));
    });
  });
}

// Class to test
class Calculator {
  int add(int a, int b) => a + b;
  int subtract(int a, int b) => a - b;
  int multiply(int a, int b) => a * b;
  double divide(int a, int b) {
    if (b == 0) throw ArgumentError('Cannot divide by zero');
    return a / b;
  }
}

// Test class
void calculatorTests() {
  group('Calculator', () {
    late Calculator calculator;
    
    setUp(() {
      calculator = Calculator();
    });
    
    test('addition works correctly', () {
      expect(calculator.add(2, 3), equals(5));
    });
    
    test('division by zero throws error', () {
      expect(() => calculator.divide(10, 0), throwsArgumentError);
    });
  });
}
```

**Interview Tips:**
- Testing code quality improve করে
- Unit tests individual functions test করে
- setUp method test setup-এর জন্য
- expect function assertions verify করে
