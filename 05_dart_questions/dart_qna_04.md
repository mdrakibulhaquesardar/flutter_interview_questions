# Dart Language - প্রশ্নোত্তর সেট ৪

## Advanced Programming Concepts

### প্রশ্ন ১৪: Dart-এ Design Patterns কীভাবে implement করবেন?

**উত্তর (ডিটেইল):**

- **Design Patterns** হলো common programming problems-এর reusable solutions। এটা code maintainability এবং reusability improve করে।

**Common Design Patterns:**
- Singleton Pattern
- Factory Pattern
- Observer Pattern
- Strategy Pattern
- Builder Pattern

**উদাহরণ:**

```dart
// Singleton Pattern
class DatabaseConnection {
  static DatabaseConnection? _instance;
  static DatabaseConnection get instance {
    _instance ??= DatabaseConnection._internal();
    return _instance!;
  }
  
  DatabaseConnection._internal();
  
  void connect() => print('Connected to database');
}

// Factory Pattern
abstract class Animal {
  void makeSound();
}

class Dog implements Animal {
  @override
  void makeSound() => print('Woof!');
}

class Cat implements Animal {
  @override
  void makeSound() => print('Meow!');
}

class AnimalFactory {
  static Animal createAnimal(String type) {
    switch (type.toLowerCase()) {
      case 'dog':
        return Dog();
      case 'cat':
        return Cat();
      default:
        throw ArgumentError('Unknown animal type: $type');
    }
  }
}

// Observer Pattern
abstract class Observer {
  void update(String message);
}

class Subject {
  final List<Observer> _observers = [];
  
  void attach(Observer observer) => _observers.add(observer);
  void detach(Observer observer) => _observers.remove(observer);
  
  void notify(String message) {
    for (final observer in _observers) {
      observer.update(message);
    }
  }
}

class ConcreteObserver implements Observer {
  final String name;
  ConcreteObserver(this.name);
  
  @override
  void update(String message) {
    print('$name received: $message');
  }
}

// Usage
void designPatternsDemo() {
  // Singleton
  final db1 = DatabaseConnection.instance;
  final db2 = DatabaseConnection.instance;
  print('Same instance: ${identical(db1, db2)}');
  
  // Factory
  final dog = AnimalFactory.createAnimal('dog');
  final cat = AnimalFactory.createAnimal('cat');
  dog.makeSound();
  cat.makeSound();
  
  // Observer
  final subject = Subject();
  final observer1 = ConcreteObserver('Observer 1');
  final observer2 = ConcreteObserver('Observer 2');
  
  subject.attach(observer1);
  subject.attach(observer2);
  subject.notify('Hello observers!');
}
```

**Interview Tips:**
- Design patterns code structure improve করে
- Singleton global state manage করে
- Factory objects create করার জন্য
- Observer loose coupling provide করে

---

### প্রশ্ন ১৫: Dart-এ Functional Programming কীভাবে implement করবেন?

**উত্তর (ডিটেইল):**

- **Functional Programming** হলো programming paradigm যা functions-কে first-class citizens হিসেবে treat করে। এটা immutable data এবং pure functions emphasize করে।

**Functional Programming Features:**
- First-class functions
- Higher-order functions
- Pure functions
- Immutability
- Function composition

**উদাহরণ:**

```dart
// Pure function
int add(int a, int b) => a + b;

// Higher-order function
List<int> mapList(List<int> list, int Function(int) mapper) {
  return list.map(mapper).toList();
}

// Function composition
int Function(int) compose(int Function(int) f, int Function(int) g) {
  return (int x) => f(g(x));
}

// Immutable data structures
class ImmutableList<T> {
  final List<T> _items;
  
  const ImmutableList(this._items);
  
  ImmutableList<T> add(T item) {
    final newItems = List<T>.from(_items)..add(item);
    return ImmutableList(newItems);
  }
  
  ImmutableList<T> remove(T item) {
    final newItems = List<T>.from(_items)..remove(item);
    return ImmutableList(newItems);
  }
  
  List<T> get items => List.unmodifiable(_items);
}

// Functional utilities
class FunctionalUtils {
  static T? find<T>(List<T> list, bool Function(T) predicate) {
    try {
      return list.firstWhere(predicate);
    } catch (e) {
      return null;
    }
  }
  
  static List<T> filter<T>(List<T> list, bool Function(T) predicate) {
    return list.where(predicate).toList();
  }
  
  static R reduce<T, R>(List<T> list, R Function(R, T) reducer, R initial) {
    R result = initial;
    for (final item in list) {
      result = reducer(result, item);
    }
    return result;
  }
}

// Usage
void functionalProgrammingDemo() {
  // Pure functions
  print('Add: ${add(5, 3)}');
  
  // Higher-order functions
  final numbers = [1, 2, 3, 4, 5];
  final doubled = mapList(numbers, (n) => n * 2);
  print('Doubled: $doubled');
  
  // Function composition
  final addOne = (int x) => x + 1;
  final multiplyByTwo = (int x) => x * 2;
  final composed = compose(addOne, multiplyByTwo);
  print('Composed: ${composed(5)}'); // (5 * 2) + 1 = 11
  
  // Immutable data
  final immutableList = ImmutableList([1, 2, 3]);
  final newList = immutableList.add(4);
  print('Original: ${immutableList.items}');
  print('New: ${newList.items}');
  
  // Functional utilities
  final evenNumbers = FunctionalUtils.filter(numbers, (n) => n % 2 == 0);
  print('Even numbers: $evenNumbers');
  
  final sum = FunctionalUtils.reduce(numbers, (sum, n) => sum + n, 0);
  print('Sum: $sum');
}
```

**Interview Tips:**
- Pure functions same input-এ same output দেয়
- Higher-order functions functions-কে parameters হিসেবে নেয়
- Immutability side effects prevent করে
- Function composition complex operations combine করে

---

### প্রশ্ন ১৬: Dart-এ Reflection এবং Metadata কীভাবে ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- **Reflection** হলো runtime-এ code-এর structure examine করার জন্য ব্যবহৃত mechanism। **Metadata** হলো code-এ additional information add করার জন্য ব্যবহৃত annotations।

**Reflection Features:**
- Runtime type information
- Dynamic method invocation
- Property access
- Class inspection

**উদাহরণ:**

```dart
import 'dart:mirrors';

// Custom metadata
class ApiEndpoint {
  final String path;
  final String method;
  
  const ApiEndpoint(this.path, this.method);
}

class ValidationRule {
  final String rule;
  final String message;
  
  const ValidationRule(this.rule, this.message);
}

// Class with metadata
class UserController {
  @ApiEndpoint('/users', 'GET')
  @ValidationRule('required', 'User ID is required')
  List<User> getUsers(String userId) {
    // Implementation
    return [];
  }
  
  @ApiEndpoint('/users', 'POST')
  User createUser(@ValidationRule('email', 'Invalid email') String email) {
    // Implementation
    return User(email: email);
  }
}

class User {
  final String email;
  User({required this.email});
}

// Reflection usage
void reflectionDemo() {
  // Get class information
  final mirror = reflectClass(UserController);
  print('Class name: ${mirror.simpleName}');
  
  // Get methods with metadata
  for (final method in mirror.declarations.values) {
    if (method is MethodMirror) {
      print('Method: ${method.simpleName}');
      
      // Get metadata
      for (final metadata in method.metadata) {
        if (metadata.type.reflectedType == ApiEndpoint) {
          final path = metadata.getField(#path).reflectee;
          final methodType = metadata.getField(#method).reflectee;
          print('  API: $methodType $path');
        }
      }
    }
  }
}

// Runtime type checking
void runtimeTypeDemo() {
  final user = User(email: 'test@example.com');
  
  // Check type at runtime
  if (user.runtimeType == User) {
    print('User is of type User');
  }
  
  // Get type information
  print('Type: ${user.runtimeType}');
  print('Type name: ${user.runtimeType.toString()}');
  
  // Check if implements interface
  print('Is Object: ${user is Object}');
  print('Is String: ${user is String}');
}

// Dynamic invocation
void dynamicInvocationDemo() {
  final user = User(email: 'test@example.com');
  
  // Dynamic property access (not recommended in production)
  try {
    final email = (user as dynamic).email;
    print('Email: $email');
  } catch (e) {
    print('Error accessing property: $e');
  }
}
```

**Interview Tips:**
- Reflection runtime performance impact করে
- Metadata compile-time information provide করে
- Runtime type checking type safety ensure করে
- Dynamic invocation type safety bypass করে

---

### প্রশ্ন ১৭: Dart-এ Serialization এবং Deserialization কীভাবে implement করবেন?

**উত্তর (ডিটেইল):**

- **Serialization** হলো objects-কে data format-এ convert করার process। **Deserialization** হলো data format থেকে objects create করার process।

**Serialization Methods:**
- JSON serialization
- XML serialization
- Custom serialization
- Code generation

**উদাহরণ:**

```dart
import 'dart:convert';

// Basic serialization
class Person {
  final String name;
  final int age;
  final String? email;
  
  Person({required this.name, required this.age, this.email});
  
  // Manual serialization
  Map<String, dynamic> toJson() {
    return {
      'name': name,
      'age': age,
      if (email != null) 'email': email,
    };
  }
  
  // Manual deserialization
  factory Person.fromJson(Map<String, dynamic> json) {
    return Person(
      name: json['name'] as String,
      age: json['age'] as int,
      email: json['email'] as String?,
    );
  }
  
  // JSON string serialization
  String toJsonString() => jsonEncode(toJson());
  
  // JSON string deserialization
  factory Person.fromJsonString(String jsonString) {
    final json = jsonDecode(jsonString) as Map<String, dynamic>;
    return Person.fromJson(json);
  }
}

// Complex object serialization
class Address {
  final String street;
  final String city;
  final String country;
  
  Address({required this.street, required this.city, required this.country});
  
  Map<String, dynamic> toJson() => {
    'street': street,
    'city': city,
    'country': country,
  };
  
  factory Address.fromJson(Map<String, dynamic> json) => Address(
    street: json['street'] as String,
    city: json['city'] as String,
    country: json['country'] as String,
  );
}

class Employee extends Person {
  final String employeeId;
  final Address address;
  final List<String> skills;
  
  Employee({
    required super.name,
    required super.age,
    required this.employeeId,
    required this.address,
    required this.skills,
    super.email,
  });
  
  @override
  Map<String, dynamic> toJson() {
    return {
      ...super.toJson(),
      'employeeId': employeeId,
      'address': address.toJson(),
      'skills': skills,
    };
  }
  
  factory Employee.fromJson(Map<String, dynamic> json) {
    return Employee(
      name: json['name'] as String,
      age: json['age'] as int,
      employeeId: json['employeeId'] as String,
      address: Address.fromJson(json['address'] as Map<String, dynamic>),
      skills: List<String>.from(json['skills'] as List),
      email: json['email'] as String?,
    );
  }
}

// Generic serialization
class Serializer<T> {
  final T Function(Map<String, dynamic>) fromJson;
  final Map<String, dynamic> Function(T) toJson;
  
  Serializer({required this.fromJson, required this.toJson});
  
  String serialize(T object) => jsonEncode(toJson(object));
  T deserialize(String jsonString) {
    final json = jsonDecode(jsonString) as Map<String, dynamic>;
    return fromJson(json);
  }
}

// Usage
void serializationDemo() {
  // Basic serialization
  final person = Person(name: 'John', age: 30, email: 'john@example.com');
  final json = person.toJson();
  print('Person JSON: $json');
  
  final jsonString = person.toJsonString();
  print('Person JSON String: $jsonString');
  
  final restoredPerson = Person.fromJsonString(jsonString);
  print('Restored person: ${restoredPerson.name}');
  
  // Complex object serialization
  final address = Address(
    street: '123 Main St',
    city: 'Dhaka',
    country: 'Bangladesh',
  );
  
  final employee = Employee(
    name: 'Jane',
    age: 25,
    employeeId: 'EMP001',
    address: address,
    skills: ['Dart', 'Flutter', 'Dart'],
  );
  
  final employeeJson = employee.toJson();
  print('Employee JSON: $employeeJson');
  
  final restoredEmployee = Employee.fromJson(employeeJson);
  print('Restored employee: ${restoredEmployee.name}');
  
  // Generic serializer
  final personSerializer = Serializer<Person>(
    fromJson: Person.fromJson,
    toJson: (person) => person.toJson(),
  );
  
  final serialized = personSerializer.serialize(person);
  final deserialized = personSerializer.deserialize(serialized);
  print('Generic serialization: ${deserialized.name}');
}
```

**Interview Tips:**
- Manual serialization control provide করে
- JSON most common format
- Nested objects careful handling দরকার
- Generic serializers reusable code provide করে

---

### প্রশ্ন ১৮: Dart-এ Performance Optimization কীভাবে implement করবেন?

**উত্তর (ডিটেইল):**

- **Performance Optimization** হলো code-এর execution speed এবং memory usage improve করার process। এটা profiling এবং optimization techniques ব্যবহার করে।

**Optimization Areas:**
- Memory management
- Algorithm efficiency
- Data structures
- Caching strategies
- Lazy loading

**উদাহরণ:**

```dart
// Memory optimization
class OptimizedList<T> {
  final List<T> _items;
  final int _maxSize;
  
  OptimizedList(this._maxSize) : _items = [];
  
  void add(T item) {
    if (_items.length >= _maxSize) {
      _items.removeAt(0); // Remove oldest item
    }
    _items.add(item);
  }
  
  List<T> get items => List.unmodifiable(_items);
}

// Caching strategy
class Cache<K, V> {
  final Map<K, V> _cache = {};
  final int _maxSize;
  
  Cache(this._maxSize);
  
  V? get(K key) => _cache[key];
  
  void put(K key, V value) {
    if (_cache.length >= _maxSize) {
      final firstKey = _cache.keys.first;
      _cache.remove(firstKey);
    }
    _cache[key] = value;
  }
  
  void clear() => _cache.clear();
}

// Lazy loading
class LazyLoader<T> {
  T? _value;
  T Function() _loader;
  
  LazyLoader(this._loader);
  
  T get value {
    _value ??= _loader();
    return _value!;
  }
  
  void reset() => _value = null;
}

// Efficient algorithms
class AlgorithmOptimizer {
  // Efficient search in sorted list
  static int binarySearch<T extends Comparable<T>>(List<T> list, T target) {
    int left = 0;
    int right = list.length - 1;
    
    while (left <= right) {
      int mid = (left + right) ~/ 2;
      int comparison = list[mid].compareTo(target);
      
      if (comparison == 0) return mid;
      if (comparison < 0) left = mid + 1;
      else right = mid - 1;
    }
    
    return -1;
  }
  
  // Efficient sorting (quick sort)
  static List<T> quickSort<T extends Comparable<T>>(List<T> list) {
    if (list.length <= 1) return list;
    
    final pivot = list[list.length ~/ 2];
    final less = <T>[];
    final equal = <T>[];
    final greater = <T>[];
    
    for (final item in list) {
      final comparison = item.compareTo(pivot);
      if (comparison < 0) {
        less.add(item);
      } else if (comparison == 0) {
        equal.add(item);
      } else {
        greater.add(item);
      }
    }
    
    return [...quickSort(less), ...equal, ...quickSort(greater)];
  }
  
  // Efficient filtering with early termination
  static List<T> filterWithLimit<T>(
    List<T> list,
    bool Function(T) predicate,
    int limit,
  ) {
    final result = <T>[];
    for (final item in list) {
      if (result.length >= limit) break;
      if (predicate(item)) result.add(item);
    }
    return result;
  }
}

// Usage
void performanceOptimizationDemo() {
  // Memory optimization
  final optimizedList = OptimizedList<String>(3);
  optimizedList.add('Item 1');
  optimizedList.add('Item 2');
  optimizedList.add('Item 3');
  optimizedList.add('Item 4'); // Removes 'Item 1'
  print('Optimized list: ${optimizedList.items}');
  
  // Caching
  final cache = Cache<String, int>(2);
  cache.put('key1', 1);
  cache.put('key2', 2);
  cache.put('key3', 3); // Removes 'key1'
  print('Cache size: ${cache._cache.length}');
  
  // Lazy loading
  final lazyLoader = LazyLoader(() {
    print('Loading expensive resource...');
    return 'Expensive Data';
  });
  
  print('First access: ${lazyLoader.value}');
  print('Second access: ${lazyLoader.value}'); // No loading message
  
  // Algorithm optimization
  final numbers = [5, 2, 8, 1, 9, 3];
  final sorted = AlgorithmOptimizer.quickSort(numbers);
  print('Sorted: $sorted');
  
  final found = AlgorithmOptimizer.binarySearch(sorted, 8);
  print('Found 8 at index: $found');
  
  final filtered = AlgorithmOptimizer.filterWithLimit(
    numbers,
    (n) => n > 3,
    3,
  );
  print('Filtered (limit 3): $filtered');
}
```

**Interview Tips:**
- Profiling identify bottlenecks
- Memory leaks avoid করুন
- Efficient algorithms use করুন
- Caching performance improve করে
- Lazy loading memory save করে
